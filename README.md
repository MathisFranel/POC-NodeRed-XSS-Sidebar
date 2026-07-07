# Stored XSS via Node Import Context

A **Stored Cross-Site Scripting (XSS)** vulnerability was identified in Node-RED's core node-import mechanism. This flaw allows an attacker to inject arbitrary JavaScript payloads that persist in the application and execute in the context of an administrative session.

> 🛡️ **Status:** Fixed
> 🔗 **Official Fix:** [Node-RED Pull Request #5597](https://github.com/node-red/node-red/pull/5597/changes/912168ccddcdf74e7fe46d89dd6d3763b109603f)

---

## 📝 Description

When importing a node configuration layout from a JSON array, the value provided in the `id` field is dynamically inserted into the HTML `class` attribute within the Config Sidebar UI component. 

Because the `id` value was not properly sanitized or HTML-escaped prior to DOM insertion, it is possible to break out of the HTML attribute string using a double quote (`"`) character. Once escaped, an attacker can append new HTML tags or event handlers (such as `onerror`) to achieve arbitrary script execution.

---

## 🚀 Proof of Concept (PoC)

To reproduce and verify the execution flow:

1. Copy the following malicious JSON payload:

```json
[
    {
        "id": "\"><img src=x onerror=alert('Stored_XSS_in_Config_Sidebar')>",
        "type": "http proxy",
        "url": {}
    }
]

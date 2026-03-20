
Vulnerability Report: Multi-Step Process with Missing Access Control

📌 Summary

A broken access control vulnerability was identified in a multi-step process where one of the steps does not properly validate user authorization. This allows an attacker to bypass security checks and perform actions they should not be permitted to execute.

🎯 Vulnerability Type

Broken Access Control

Insecure Multi-Step Process

📍 Affected Functionality

Multi-step workflow (e.g., account update / admin action)

Specifically: One step lacks proper server-side authorization checks

🧪 Steps to Reproduce

Log in as a normal user (e.g., wiener)

Start the multi-step process (e.g., account change or sensitive action)

Intercept the requests using a proxy tool (like Burp Suite)

Observe multiple steps (Step 1 → Step 2 → Step 3)

Identify the step that enforces access control (e.g., admin check)

Skip or manipulate that step

Send the request directly to the vulnerable step without proper authorization

Notice that the action is still processed successfully

⚠️ Impact

Unauthorized users can perform restricted actions

Privilege escalation may be possible

Sensitive operations can be executed without proper validation

Compromise of user accounts or administrative functionality

🧠 Root Cause

The application:

Enforces access control in only one step

Fails to validate authorization in subsequent steps

Assumes earlier steps were completed legitimately (trusts client-side flow)

🛠️ Proof of Concept (Example)
POST /admin/update-role HTTP/1.1
Host: vulnerable-site.com
Cookie: session=normal-user-session

username=carlos&role=admin

➡️ Even without proper authorization, the request succeeds because the server does not validate permissions at this step.

🛡️ Recommended Fix

Enforce server-side access control checks on every step

Do not rely on previous steps for authorization

Validate user roles and permissions before processing any sensitive request

Implement centralized authorization logic

Use secure session validation

🔍 Severity

High

📚 References

OWASP Top 10: Broken Access Control

Principle of Least Privilege

✅ Conclusion

This vulnerability demonstrates how improper validation in multi-step processes can lead to serious security issues. Each step in a workflow must independently verify authorization to prevent attackers from bypassing restrictions.

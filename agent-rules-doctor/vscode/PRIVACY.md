# AgentRules Doctor for VS Code — privacy draft

MOON LABORATORIES PTY LTD (ACN 703 101 384; ABN 45 703 101 384) provides the Visual Studio Code extension. This statement covers the extension, not the separate JetBrains plugin.

The extension reads supported instruction files and nearby configuration within each open workspace root. It uses the content locally to calculate findings and diagnostics. The scanner makes no network request, runs no repository commands, uploads no instruction text, paths, or findings, and sends no telemetry. It does not need an account or AI service. Findings stay in the open editor session. Scanning does not modify files. If you invoke Create ignore file in a trusted local workspace, the extension creates `.agentrules-doctor-ignore`; it does not overwrite an existing file. You may edit or remove it.

There is no purchase, activation, or licensing request in this VS Code release. VS Code, Marketplace, other extensions, and your operating system may independently use network services under their own policies. Opening an external documentation link can connect your browser to that site.

If you contact support, Moon Laboratories receives the email and information you choose to provide to answer your request. Do not send secrets or private project text. Remove sensitive details from screenshots and logs. Contact karuppaiah.al@gmail.com for privacy requests.

This is a release draft awaiting legal and installed-behaviour review. Remote and virtual workspace support has not been validated.

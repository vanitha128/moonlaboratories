# AgentRules Doctor for VS Code — customer guide

This guide covers the local desktop VS Code extension. Only Free checks are available. Pro, purchases, trials, and license activation are not available. The extension is not yet in the public Marketplace.

## Install a supplied VSIX

1. Obtain the installable `.vsix` from Moon Laboratories.
2. Open desktop VS Code.
3. Open **Extensions**. Select **… > Install from VSIX…**.
4. Select the supplied file. Reload VS Code if asked.

## Scan a folder

1. Open a local folder that has `AGENTS.md`, `CLAUDE.md`, `.cursorrules`, a Cursor `.mdc` rule, or a GitHub Copilot instruction file.
2. Open **AgentRules Doctor** in the Activity Bar. Select **Health**, **Files**, or **Findings**.
3. Select **AgentRules Doctor: Scan Workspace** from the Command Palette to scan again.
4. Check coverage warnings before you judge the score. Select a finding and read its evidence.
5. Open the source. Confirm the problem before you edit it. Save your change and scan again.

The scanner reads files. It does not execute commands or correct instructions automatically. The Free rules check missing paths, npm scripts, build wrappers, simple Maven phases, and ignore-file syntax. The other eleven validators exist in development tests but are unavailable in this release.

## Filter findings

1. In the **Findings** view, select **Filter File** and choose a file, or **All files**.
2. Select **Filter Severity** and choose a severity or **all**.
3. Read the visible and total counts. Filters do not change the underlying score.

## Ignore a known false positive

1. In a trusted local workspace, select **AgentRules Doctor: Create Ignore File**. This creates `.agentrules-doctor-ignore` only if it does not exist.
2. Add a line such as `rule MISSING_PATH` to suppress one rule, or `file docs/AGENTS.md` to skip one supported instruction file.
3. Save the file. Rescan. Correct any `INVALID_IGNORE_ENTRY` warning.

An ignore path must name an existing supported instruction file inside the workspace. Do not use ignore entries to hide problems you have not checked.

## Score and limits

The score starts at 100. Each error removes 10 points, each warning 4, and each information finding 1. The minimum is 0. It uses enabled, unsuppressed findings. A score of 100 does not prove correctness or security. Coverage is partial when the scan reaches 20,000 visited files or cannot read paths. Files larger than 512 KiB for instructions, 64 KiB for ignore settings, or 1 MiB for project configuration may be skipped. Use a local desktop workspace; remote and virtual workspaces remain untested.

For assistance, see [Support](SUPPORT.md). Do not send secrets or private project text. Provider: MOON LABORATORIES PTY LTD (ACN 703 101 384; ABN 45 703 101 384), karuppaiah.al@gmail.com.

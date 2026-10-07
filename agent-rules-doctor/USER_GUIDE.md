# AgentRules Doctor User Guide

Version: Development build 0.1.0  
Last update: 2026-10-07

AgentRules Doctor is a JetBrains plugin. It checks instruction files for coding agents in your project. The plugin shows findings in the **Agent Rules** tool window.

This guide describes the current development build. The plugin is not yet on JetBrains Marketplace. Paid features and license checks are not active.

## Before you start

- Use a local project in IntelliJ IDEA 2025.3.6.1. This is the IDE version that we tested.
- If you have source access, get the development ZIP from the project build. See [Build the plugin](#build-the-plugin).
- Keep a copy of your instruction files before you change them.

The plugin reads instruction files. It does not change them during a scan. The plugin does not run project commands.

## Build the plugin

Use this procedure only if you have access to the private plugin source repository. Use JDK 21 to build the development version.

1. Open a terminal in the [plugin source repository](https://github.com/foryforx/agent-rules-doctor).
2. Run `./gradlew test buildPlugin`.
3. Find the ZIP in `build/distributions/`.

## Install the development build

1. Open IntelliJ IDEA.
2. Open **Settings** or **Preferences**.
3. Select **Plugins**.
4. Open the plugin menu. Select **Install Plugin from Disk**.
5. Select the ZIP in `build/distributions/`.
6. Restart the IDE if it asks you to restart.

The build is for tests. Do not use it as a Marketplace release.

## Install a Marketplace release

This option will be available after JetBrains publishes the plugin. The development build is not on Marketplace.

1. Open IntelliJ IDEA.
2. Open **Settings** or **Preferences**.
3. Select **Plugins**.
4. Search Marketplace for **AgentRules Doctor**.
5. Select **Install**.
6. Restart the IDE if it asks you to restart.

## Scan a project

1. Open a local project in IntelliJ IDEA.
2. Select **View > Tool Windows > Agent Rules**.
3. Wait for the scan to finish.
4. Read the status line. It shows the score, file count, and finding count.

The first scan starts when you open the tool window. Select **Rescan** after you change an instruction file or an ignore setting.

Select **User guide** to open this page in your browser. Your browser contacts GitHub. The plugin does not send project files with this action.

## Read a finding

1. Select a finding in the list.
2. Read the evidence and the suggested action below the list.
3. Double-click the finding to open its source file.
4. Check the instruction and the project configuration.
5. Correct the instruction or the project file if the finding is valid.

The plugin does not make the correction for you. Check each finding before you change a file.

## Filter findings

Use the menu next to **Rescan**. Select **All findings**, **Errors**, **Warnings**, or **Info**.

The filter changes the list. It does not change the score. The status line shows the visible count and the total count.

## Supported instruction files

The scanner looks for these files:

| File pattern | Location |
| --- | --- |
| `AGENTS.md` | Project root or a project subfolder |
| `CLAUDE.md` | Project root or a project subfolder |
| `.cursorrules` | Project root |
| `.cursor/rules/**/*.mdc` | Cursor rule folders in the project |
| `.github/copilot-instructions.md` | Directly in `.github` |
| `.github/instructions/*.instructions.md` | Directly in `.github/instructions` |

The scanner skips build output, dependencies, and other generated folders. It does not follow symbolic links.

## Checks in this build

| Check | Rule ID | What the plugin reports |
| --- | --- | --- |
| Missing path | `MISSING_PATH` | A supported literal relative path in a code span or Markdown link does not exist. |
| Missing npm script | `MISSING_NPM_SCRIPT` | An `npm run NAME` command names no script in a nearby `package.json`. |
| Missing build wrapper | `MISSING_BUILD_WRAPPER` | A code span starts with `./gradlew` or `./mvnw`, but the wrapper file does not exist. |
| Java version mismatch | `JAVA_VERSION_MISMATCH` | An explicit Java or JDK major version differs from an unambiguous project setting. |
| Node version mismatch | `NODE_VERSION_MISMATCH` | An explicit Node major version differs from `.nvmrc` or `.node-version`. |
| Repeated directive | `DUPLICATE_DIRECTIVE` | The same instruction appears in two supported files after basic text normalization. |
| Conflicting directive | `CONFLICTING_DIRECTIVE` | Files in one folder say `Always ACTION` and `Never ACTION` for the same exact action. |
| Invalid ignore entry | `INVALID_IGNORE_ENTRY` | An ignore setting contains an unknown rule, file, or instruction. |

The Java check reads a root Gradle toolchain, Maven compiler release, or `.java-version`. The Node check reads a root `.nvmrc` or `.node-version`.

The plugin reports only matches that it can check with local project data. It does not understand all natural-language instructions.

## Understand the score

The score starts at 100. Each error removes 10 points. Each warning removes 4 points. Each information finding removes 1 point. The lowest score is 0.

The score uses active findings from the scan. It is a summary of these checks. A score of 100 does not prove that all instructions are correct.

## Ignore a known false positive

Create `.agentrules-doctor-ignore` in the project root. Use one setting on each line. Use `#` at the start of a line for a comment.

```text
# Ignore one rule in this project.
rule DUPLICATE_DIRECTIVE

# Ignore one instruction file.
file generated/AGENTS.md
```

Use `rule RULE_ID` to ignore one rule across the project. Find the rule ID in the finding details. Use `file PATH` to skip one supported instruction file.

The file path must be relative to the project root. The instruction file must exist. The path cannot point outside the project.

Select **Rescan** after you save the ignore file. The plugin shows a warning if a setting is invalid. It does not silently accept an unknown rule or file.

An ignored file does not appear in the scanned file count. An ignored finding does not reduce the score.

## Limits of this build

- The scanner stops after it visits 20,000 files.
- The scanner skips instruction files larger than 512 KiB.
- The ignore file must be smaller than 64 KiB.
- The scanner does not check every command or dependency.
- The scanner does not detect all contradictions or near-duplicate instructions.
- The scanner does not run a network request or upload project data.

These limits can cause a missing finding. Check important instructions yourself.

## If the result is not as you expect

**No files appear:** Check the file names and locations in [Supported instruction files](#supported-instruction-files). Then select **Rescan**.

**A recent change does not appear:** Save the file. Then select **Rescan**.

**A finding looks incorrect:** Read its evidence. Check the path or configuration file. Use an ignore setting only if the finding is a known false positive.

**The tool window does not open:** Check that you installed and enabled the plugin. Restart IntelliJ IDEA. Then open the tool window again.

If the problem continues, use the [support page](SUPPORT.md). Send the IDE version, plugin version, and steps that show the problem. Remove secrets from logs and screenshots.

## Privacy and support

The scan runs on your computer. The plugin does not send instruction text or findings to Moon Laboratories. See the [privacy policy](PRIVACY.md).

Use the [support page](SUPPORT.md) for product help. The [license draft](LICENSE) describes proposed legal terms. Moon Laboratories will finalize these documents before Marketplace release.

JetBrains Marketplace will handle sales and license delivery if Moon Laboratories releases paid features. This development build has no purchase flow and no paid license check.

## Development status

On 2026-10-07, local tests and ZIP packaging passed. JetBrains Plugin Verifier reported compatibility with IntelliJ IDEA 2025.3.6.1.

The verifier also reported four deprecated API usages and six experimental API usages. These results apply to the tested IDE version only.

The Agent Rules tool window showed five sample findings and a 56/100 score. This test did not confirm every filter, Rescan, or source navigation action.

Paid licensing, broader IDE tests, final legal terms, and Marketplace submission are not complete.

# AgentRules Doctor User Guide

Last update: 2026-10-07

This guide is for people who install AgentRules Doctor from JetBrains Marketplace. The plugin checks agent instruction files in an open project. It shows results in the **Agent Rules** tool window.

## Install the plugin

1. Open IntelliJ IDEA.
2. Open **Settings** or **Preferences**.
3. Select **Plugins**.
4. Select the **Marketplace** tab. Search for **AgentRules Doctor**.
5. Select **Install**.
6. Restart the IDE if it asks you to restart.

Check the Marketplace listing for supported IDE versions and available plans.

## Buy and activate Pro

You can use Free checks without buying Pro. To buy Pro:

1. Open the plugin page in JetBrains Marketplace.
2. Select **Pricing**.
3. Select a plan.
4. Complete the purchase with your JetBrains Account.

JetBrains Marketplace manages the purchase and the license.

To activate Pro:

1. Open **Help > Register** in the IDE.
2. Sign in with the JetBrains Account that you used for the purchase.
3. Select the plugin license.
4. Select **Activate**.

If you start a trial, follow the trial prompt in the IDE.

The status line shows **Pro active** when the IDE confirms Pro access. If the license is not on the list, select **Refresh license list** in the IDE license window. If the plugin still shows **Free checks**, select **Refresh license** in the plugin. This checks the license again and scans the project. If the plugin shows **License status pending**, wait for the IDE to start. Then select **Refresh license**.

If activation fails, check that you use the correct JetBrains Account. For purchase or license delivery problems, contact JetBrains Marketplace. For a plugin problem, use the [support page](SUPPORT.md).

## Scan a project

1. Open a local project in IntelliJ IDEA.
2. Select **View > Tool Windows > Agent Rules**.
3. Wait for the scan to finish.
4. Read the **Health** view. It shows the score, file count, and finding count.

The plugin starts a background scan when you open a project. Open the tool window to see the result. If the scan is still running, wait for it to finish. Select **Rescan** after you change an instruction file or an ignore setting. The plugin reads files during a scan. It does not change files or run project commands during the scan.

Select **User guide** to open this page in your browser. The plugin does not include project files in the link.

## Use the views

- **Health** shows the score, coverage, finding counts, and top findings. Select a top finding and then select **View finding** to see its details.
- **Files** lists each scanned instruction file with a score and finding count. Select a file to see its findings. Select **Show findings** to filter the Findings view to that file. Select **Open file** to open it in the editor.
- **Findings** shows the finding list, evidence, and suggested action.
- **Duplicates & conflicts** shows repeated or conflicting instructions. Select a finding to compare the scanned source line with related evidence. Select **Open source** to inspect the file in the editor. Select **Rescan** after you change the file. This view has results only when a Pro license is active.
- **Settings** shows license status, available checks, and the local ignore settings file. Select **Create ignore file** if the file does not exist. Select **Open ignore file** to edit an existing file.

The score for one file uses findings that point to that file. The project score uses all findings available with your license.

## Read a finding

1. Select **Findings**, then select a finding in the list.
2. Read the evidence and the suggested action below the list.
3. Select **Open source** to open the source file. You can also double-click the finding.
4. Check the instruction and the project configuration.
5. Correct the instruction or the project file if the finding is valid.

The plugin does not make the correction for you. Keep a copy of your files before you change them. Check each finding before you change a file.

## Filter findings

In **Findings**, open the file menu. Select an instruction file to show its findings. Select **All files** to show findings from every scanned file.

Use the other menu to select **All findings**, **Errors**, **Warnings**, or **Info**. You can use both menus together.

The filters change the list. They do not change the score. The status line shows the visible count and the total count. The file menu keeps your selection after a rescan if that file is still in the project.

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

## Checks

| Check | Rule ID | What the plugin reports |
| --- | --- | --- |
| Missing path | `MISSING_PATH` | A supported literal relative path in a code span or Markdown link does not exist. The check skips ambiguous examples and import names. |
| Missing npm script | `MISSING_NPM_SCRIPT` | An `npm run NAME` command names no script in a nearby `package.json`. |
| Undeclared npm dependency | `UNDECLARED_NPM_DEPENDENCY` | An instruction explicitly says that `package.json` lists a package as a dependency, but the nearest manifest does not list it. |
| Undeclared Maven dependency | `UNDECLARED_MAVEN_DEPENDENCY` | An instruction explicitly says that `pom.xml` lists a named dependency, but the nearest readable POM does not list it. |
| Missing build wrapper | `MISSING_BUILD_WRAPPER` | A code span starts with `./gradlew` or `./mvnw`, but the wrapper file does not exist. |
| Unknown Maven phase | `UNKNOWN_MAVEN_PHASE` | A simple quoted `mvn` or `./mvnw` command names a phase that Maven does not provide. The check needs a nearby `pom.xml`. |
| Gradle task registration not found | `UNREGISTERED_GRADLE_TASK` | An instruction says that `build.gradle` or `build.gradle.kts` registers a task. The nearest named file has no matching literal registration. |
| Java version mismatch | `JAVA_VERSION_MISMATCH` | An explicit Java or JDK major version differs from an unambiguous project setting. |
| Node version mismatch | `NODE_VERSION_MISMATCH` | An explicit Node major version differs from `.nvmrc` or `.node-version`. |
| Python version mismatch | `PYTHON_VERSION_MISMATCH` | An explicit Python major or minor version differs from `.python-version`. |
| Repeated directive | `DUPLICATE_DIRECTIVE` | The same instruction appears in two supported files after basic text normalization. |
| Nearly repeated directive | `NEAR_DUPLICATE_DIRECTIVE` | Two plain-text instructions in files in one folder differ only by `a`, `an`, or `the`. |
| Repeated context | `DUPLICATE_CONTEXT` | A substantial paragraph appears in two supported files. The finding includes a rough estimate of repeated tokens. |
| Conflicting directive | `CONFLICTING_DIRECTIVE` | Files in one folder say `Always ACTION` and `Never ACTION` for the same exact action. |
| Invalid ignore entry | `INVALID_IGNORE_ENTRY` | An ignore setting contains an unknown rule, file, or instruction. |

Missing path, missing npm script, missing build wrapper, unknown Maven phase, and invalid ignore entry checks are Free. The other checks require Pro access. When the status says **Free checks**, the list and score include only Free findings.

The npm dependency check uses explicit statements such as ``package.json lists `react` as a dependency``. It reads the nearest `package.json`. It does not treat an instruction to install a package as a claim that the package is already declared.

The npm checks read the top-level sections of a valid `package.json`. If the file is not valid JSON, the plugin skips these checks for that file.

The Maven dependency check uses explicit statements such as ``pom.xml lists `org.example:example-lib` as a dependency``. It reads the nearest `pom.xml`. It skips a POM that it cannot read or that uses a property in a dependency name.

The path check skips fenced examples, lines marked as examples or conventions, package imports, path aliases, image tags, and unqualified directory names. For a Cursor rule with one clear file scope, it also checks paths from that scoped folder. A parent folder can contain several separate projects. For more accurate path results, open each project at its own root.

The Java check reads a root Gradle toolchain, Maven compiler release, or `.java-version`. The Node check reads a root `.nvmrc` or `.node-version`. The Python check reads a root `.python-version`.

The Gradle check looks for explicit statements such as ``build.gradle.kts registers `verifyRules` as a task``. It does not check ordinary `gradlew` commands because plugins can create tasks. If a task comes from a plugin or another build file, check the evidence before you change the instruction.

The token estimate uses four characters per token. Actual token counts vary. The duplicate and conflict checks skip fenced code examples. The Maven check does not inspect commands with options or plugin goals. The plugin reports only matches that it can check with local project data. It does not understand all natural-language instructions.

## Understand the score

The score starts at 100. Each error removes 10 points. Each warning removes 4 points. Each information finding removes 1 point. The lowest score is 0.

The score uses active findings from the scan. It is a summary of these checks. A score of 100 does not prove that all instructions are correct.

## Ignore a known false positive

In **Settings**, select **Create ignore file** if the file does not exist. The plugin creates `.agentrules-doctor-ignore` in the project root. It opens the file in the editor. If the file exists, select **Open ignore file**. Use one setting on each line. Use `#` at the start of a line for a comment.

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

## Limits

- The scanner stops after it visits 20,000 files. The status line says **Partial scan** if it reaches this limit. The score then covers only scanned files.
- The scanner skips a folder or instruction file that it cannot read. It marks the scan as partial. The score then covers only readable files.
- The scanner skips instruction files larger than 512 KiB.
- The ignore file must be smaller than 64 KiB.
- The scanner skips project configuration files larger than 1 MiB. It does not follow symbolic links to project configuration files or build wrappers.
- The scanner does not check every command or dependency.
- The scanner does not detect all contradictions or near-duplicate instructions.
- The scanner does not run a network request or upload project data.

These limits can cause a missing finding. Check important instructions yourself.

## If the result is not as you expect

**No files appear:** Check the file names and locations in [Supported instruction files](#supported-instruction-files). If the instruction file is inside a separate child project, open that child project in the IDE. Then select **Rescan**.

**A recent change does not appear:** Save the file. Then select **Rescan**.

**A finding looks incorrect:** Read its evidence. Check the path or configuration file. Use an ignore setting only if the finding is a known false positive.

**The tool window does not open:** Check that you installed and enabled the plugin. Restart IntelliJ IDEA. Then open the tool window again.

If the problem continues, use the [support page](SUPPORT.md). Send the IDE version, plugin version, and steps that show the problem. Remove secrets from logs and screenshots.

## Privacy and support

The scan runs on your computer. The plugin does not send instruction text or findings to Moon Laboratories. See the [privacy policy](PRIVACY.md).

Use the [support page](SUPPORT.md) for product help. JetBrains Marketplace manages purchases and license delivery.

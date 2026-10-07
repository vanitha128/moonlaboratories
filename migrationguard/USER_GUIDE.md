# MigrationGuard User Guide

**For:** IntelliJ IDEA users who install MigrationGuard from JetBrains Marketplace

**Last update:** 2026-10-07

MigrationGuard checks PostgreSQL Flyway SQL files in your open project. It shows changes that need review before deployment. You can use the Free checks without a license. A trial or Pro subscription adds four checks.

**Availability:** The Marketplace listing is under preparation. The install and purchase steps below apply when JetBrains approves the listing.

## Install MigrationGuard

1. Open IntelliJ IDEA 2025.3 or a later compatible version.
2. Open **Settings** on Windows or Linux, or **Preferences** on macOS.
3. Select **Plugins > Marketplace**.
4. Search for **MigrationGuard** by **Moon Laboratories**.
5. Select **Install**.
6. Restart the IDE if it asks you to restart.

You can use the Free checks as soon as installation is complete. To confirm the installation, open **Plugins > Installed** and check that MigrationGuard is enabled.

![MigrationGuard enabled in the Installed plugins list.](images/installed.jpg)

*Figure 1. MigrationGuard appears in the Installed plugins list.*

For more help with installation, see the [IntelliJ IDEA plugin guide](https://www.jetbrains.com/help/idea/managing-plugins.html).

## Scan your migrations

1. Open the project that contains your Flyway SQL files.
2. Select **View > Tool Windows > MigrationGuard**.
3. Select **Scan project**.
4. Wait for the scan to finish.
5. Read the finding count above the list.

MigrationGuard scans versioned files with names such as `V2__add_accounts.sql`. It does not scan repeatable migrations with names such as `R__refresh_view.sql`.

![MigrationGuard scan results in IntelliJ IDEA.](images/scan-results.jpg)

*Figure 2. Example results when all checks are active. Your results depend on your SQL files and license.*

## Review a finding

1. Select a finding in the list.
2. Read **Why risky** and **Safer pattern** below the list.
3. Check the SQL and your deployment plan.
4. Double-click the finding to open its SQL file at the reported location.

A data-source message in the SQL editor comes from the IDE. MigrationGuard does not need a database connection.

## Use a trial or Pro subscription

1. Select **Unlock Pro** in the MigrationGuard tool window.
2. Follow the JetBrains license window to start a trial or activate a subscription. You can also buy Pro from the MigrationGuard Marketplace page when sales are available.
3. Sign in with the JetBrains Account that has the trial or subscription.
4. Select **Scan project** again. The scan now includes Pro checks.

If you bought Pro and it does not activate, open **Help > Register** in the IDE. Sign in with the account used for the purchase, refresh the license list, and activate the MigrationGuard license. Then scan again. See the [JetBrains license guide](https://www.jetbrains.com/help/idea/register.html) for account and activation steps.

If a trial or subscription ends, the Free checks remain available. JetBrains Marketplace manages trial and subscription billing.

## Know which checks you have

| Rule | Access | What it finds |
| --- | --- | --- |
| `MG001` | Free | `DROP TABLE` deletes a table. |
| `MG002` | Free | `DROP COLUMN` deletes a column. |
| `MG005` | Free | Two files in one migration directory use the same numeric Flyway version. |
| `MG003` | Pro | `RENAME` changes a table or column name. |
| `MG004` | Pro | `CREATE INDEX` does not use `CONCURRENTLY`. |
| `MG006` | Pro | A new `NOT NULL` column has no default. |
| `MG007` | Pro | `UPDATE` or `DELETE` has no `WHERE` clause. |

Each finding shows a severity, rule ID, file, line, reason, and safer pattern. A **Critical** or **Warning** label identifies a risk. It does not mean that the migration will fail.

## Scan again after a change

1. Review the finding and the SQL file.
2. Check whether the migration has already run in another environment.
3. Change and save the file only if your deployment plan permits it.
4. Select **Scan project** again.

MigrationGuard does not change your SQL files. Do not edit an applied migration without a release plan.

## If you see no findings

1. Check that versioned Flyway SQL files are inside the open project.
2. Check the file names. A valid example is `V2__add_accounts.sql`.
3. Select **Scan project** again.

No findings does not prove that a migration is safe. MigrationGuard checks selected SQL patterns and cannot test your database or deployment.

## Privacy and help

MigrationGuard reads SQL files on your computer. It does not connect to a database or upload your SQL.

For help with a scan, license, or subscription, see [Support](SUPPORT.md). Read the [Privacy Policy](PRIVACY.md) and [License](LICENSE) for more information.

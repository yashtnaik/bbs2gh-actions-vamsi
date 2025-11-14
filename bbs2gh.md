# Migration Strategy: bbs2gh

The `bbs2gh` migration strategy is the **recommended approach** for migrating repositories from Bitbucket Data Center to GitHub Enterprise Cloud (GHEC), GitHub Enterprise Cloud with EMU (GHEC+EMU), or GitHub Enterprise Cloud with Data Residency (GHEC+DR) using the latest `bbs2gh` tool.

> **✅ Recommended Choice**: This is the preferred migration strategy for new Bitbucket Data Center migrations, offering improved performance and direct upload capabilities.

> **⚠️ Beta Feature**: This migration strategy uses a beta feature of the GEI `bbs2gh` tool that allows direct uploads to GitHub Enterprise Cloud without intermediate storage. Ensure proper feature flags are enabled before use.

## Key Benefits

- **🚀 Direct Upload**: No intermediate storage required, faster transfers
- **⚡ Better Performance**: Optimized for large-scale enterprise migrations
- **🔄 Latest Features**: Access to newest migration capabilities and improvements
- **🛡️ Enterprise Ready**: Full support for GitHub Enterprise Cloud features

## Quick Start

1. **Configure** variables and secrets (see tables below)
2. **Enable** beta feature flags with GitHub Support
3. **Run** Configuration Settings workflow to apply changes
4. **Submit** migration batch using [Repository Dispatch](../Repository-Dispatch.md)

> **📚 New to Migrations?** Start with the **[Complete Guide](../Complete-Guide.md)** for comprehensive setup instructions.

## Configuration Requirements

The following variables and secrets are required for the `bbs2gh` migration strategy. Update the corresponding values in the `config.yaml` file and manually add secrets to the repository secrets.

> **⚙️ Important**: Run the `Configuration Settings` workflow after updating `config.yaml` to apply changes.

### Required Variables

| Variable Name                     | Description                                                                                                         | Example                                            |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------- |
| `BITBUCKET_SERVER_URL`            | The URL of the Bitbucket Data Center instance                                                                       | `https://bitbucket.example.com`                    |
| `BITBUCKET_SHARED_HOME`           | The shared home directory on the Bitbucket Data Center instance                                                     | `/var/atlassian/application-data/bitbucket/shared` |
| `BITBUCKET_ARCHIVE_DOWNLOAD_HOST` | The host where the Bitbucket Data Center archive files are stored                                                   | `192.168.1.100`                                    |
| `BITBUCKET_SSH_USER`              | The SSH user that will be used to download the Bitbucket Data Center archive files                                  | `bitbucket`                                        |
| `BITBUCKET_SSH_PORT`              | The SSH port that will be used to download the Bitbucket Data Center archive files                                  | `2222`                                             |
| `BITBUCKET_USERNAME`              | The username of the Bitbucket Data Center user that will be used to authenticate with the Bitbucket Data Center API | `bitbucket_user`                                   |

### Required Secrets

| Secret Name          | Description                                                                                                         | Example                                                               |
| -------------------- | ------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| `BBS_PASSWORD`       | The password of the Bitbucket Data Center user that will be used to authenticate with the Bitbucket Data Center API for generating Inventory | `bitbucket_pass`                                                      |
| `BITBUCKET_SSH_KEY`  | The SSH private key that will be used to download the Bitbucket Data Center archive files                           | `-----BEGIN PRIVATE KEY----- ABUNCHOFSTUFF -----END PRIVATE KEY-----` |

> **🔐 Security**: Store all secrets securely in repository settings. Never commit secrets to your repository.

## Migration Workflow Steps

The `bbs2gh` migration strategy consists of the following workflow files executed in sequence:

| Step | Workflow File                 | Purpose                                                    |
| ---- | ----------------------------- | ---------------------------------------------------------- |
| 1    | `migrate-validate.yaml`       | Validates required variables and secrets before migration  |
| 2    | `migrate-bbs-gei.yaml`        | Executes the main repository migration using `bbs2gh` tool |
| 3    | `migrate-bbs-gei-status.yaml` | Verifies migration status and validates results            |
| 4    | `migrate-lfs.yaml`            | Migrates Large File System (LFS) objects if present        |
| 5    | `post-access-restore.yaml`    | Restores repository access settings and team permissions   |

### Workflow Sequence

```mermaid
graph LR
    A[📋 Validate] --> B[🔄 BBS Migration]
    B --> C[📊 Status Check]
    C --> D[📦 LFS Migration]
    D --> E[🔐 Access Restore]

    A1[Configuration Check<br/>Credential Validation<br/>Connectivity Test] -.-> A
    B1[Repository Export<br/>Direct Upload<br/>Metadata Transfer] -.-> B
    C1[Migration Verification<br/>Quality Assurance<br/>Error Detection] -.-> C
    D1[Large File Transfer<br/>Binary Asset Migration<br/>Storage Sync] -.-> D
    E1[Team Assignment<br/>Permission Restore<br/>Security Settings] -.-> E
```

> **🛠️ Customization**: Need to modify these workflows? See the **[Customizing Migrations Guide](../Customizing-Migrations.md)** for detailed instructions.

## Prerequisites

### System Requirements

- **Bitbucket Data Center**: Compatible version with API access
- **Network Access**: Connectivity between runners and Bitbucket instance
- **SSH Access**: For archive file downloads
- **GitHub Enterprise**: Feature flags enabled for direct upload

### Required Permissions

- **Bitbucket**: Repository admin access for source repositories
- **GitHub**: Organization owner or admin access for destination
- **SSH**: Key-based authentication to Bitbucket archive storage

> **🚀 Getting Started**: Ready to begin? Follow the **[Complete Guide](../Complete-Guide.md)** for step-by-step setup instructions.

## Support and Troubleshooting

### Common Issues

- **Authentication failures**: Verify tokens and SSH keys
- **Network connectivity**: Check firewall and routing
- **Feature flag access**: Contact GitHub Support for beta features
- **Archive download issues**: Validate SSH configuration

### Resources

- **[Operations Guide](../Operations-Guide.md)** - Daily management and monitoring
- **[Troubleshooting Guide](../Troubleshooting-Guide.md)** - Comprehensive issue resolution
- **[Quick Reference](../Quick-Reference.md)** - Essential commands and procedures

### When to Use Alternative Strategies

Consider the **[bbsExport strategy](./bbsExport.md)** if:

- Beta features are not available in your environment
- You need proven, stable tooling for critical migrations
- Your compliance requirements prohibit beta feature usage

---

_This strategy guide is part of the comprehensive **[Migrations via Actions Documentation](../README.md)**. For complete deployment guidance, see the **[Complete Guide](../Complete-Guide.md)**._

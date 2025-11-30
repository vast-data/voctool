## voctool

voctool is a small command-line tool for managing VoC (VAST On Cloud) resources.

It focuses on safe, authenticated operations and supports AWS, GCP, and Azure via their official cloud SDKs and CLIs.

## Installation (from GitHub Releases)

- **Download the binary**:
  - Go to the GitHub repository Releases page for voctool.
  - Download the asset that matches your operating system and CPU architecture (for example: Linux `amd64`, macOS `arm64`, etc.).

- **Install on macOS / Linux** (adjust the filename as needed):

```bash
chmod +x voctool

# Verify
voctool --version
```

- **Install on Windows**:
  - Download the Windows binary from the Release assets.
  - Rename it to `voctool.exe` if needed.
  - Place it in a directory on your `PATH` (for example, `C:\Users\<you>\bin`) and open a new terminal.
  - Run:

```powershell
voctool --version
```

## Usage

Basic usage pattern:

```bash
voctool [command] [flags]
```

If you run `voctool` with no arguments, it prints help and a command tree.

## Commands

- **cluster-cleanup**

  Clean up cluster-related resources leftover after the VoC cluster was destroyed using terraform

  ```bash
  voctool cluster-cleanup --cluster-name <name> --cloud-provider <provider> [flags]
  ```

  - **Supported providers**:
    - `AWS`
    - `GCP`
    - `Azure`

  - **Examples**:

    - GCP (cleans up matching routes in the given project):

    ```bash
    voctool cluster-cleanup \
      --cluster-name my-cluster \
      --cloud-provider GCP \
      --project-id my-gcp-project
    ```

    - AWS (currently a no-op, reserved for future use):

    ```bash
    voctool cluster-cleanup \
      --cluster-name my-cluster \
      --cloud-provider AWS
    ```

    - Azure (currently a no-op, reserved for future use):

    ```bash
    voctool cluster-cleanup \
      --cluster-name my-cluster \
      --cloud-provider Azure \
      --subscription-id my-subscription-id
    ```

  - **Flags**:
    - `--cluster-name` (string, required): Cluster name or UID to clean up.
    - `--cloud-provider` (string, required): One of `AWS`, `GCP`, or `Azure`.
    - `--project-id` (string, required for `GCP`): GCP project ID.
    - `--subscription-id` (string, required for `Azure`): Azure subscription ID.

## Global Flags

These flags are available for all commands:

- **`--verbose`, `-v`**: Enable verbose output.
- **`--version`**: Show the voctool version.

## Authentication

voctool uses the official cloud SDKs and CLIs to authenticate. When a command requires access to a cloud provider:

- The tool automatically detects if you are authenticated.
- If authentication is missing or expired, voctool will:
  - Explain what authentication command needs to run (such as a `gcloud`, `aws`, or `az` login flow).
  - Ask for your **explicit consent** before running anything on your behalf.
- After authentication completes, the command re-validates access before changing any resources.

You do not need to manage credentials directly; just ensure the respective cloud CLIs are installed and available on your `PATH`.



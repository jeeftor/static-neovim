# Static Neovim Builder

This project builds a static binary of Neovim using GitHub Actions, making it easy to distribute and use Neovim without dependencies.

## What it does

- **Weekly builds**: Automatically builds a static Neovim binary every Monday at 2 AM UTC
- **Manual triggers**: Can also be triggered manually via GitHub Actions UI
- **Static compilation**: Creates a fully self-contained binary with no external dependencies
- **Cross-platform ready**: Built on Ubuntu but the resulting binary works on most Linux distributions

## Workflow Details

The GitHub Actions workflow consists of two jobs:

### 1. `build-static-neovim`
- Runs on Alpine Linux container
- Installs required build dependencies (cmake, build tools, etc.)
- Clones the stable branch of Neovim from GitHub
- Compiles with static linking (`CMAKE_EXTRA_FLAGS="-DSTATIC_BUILD=1"`)
- Caches the binary for efficiency
- Uploads the binary as a GitHub Actions artifact

### 2. `use-static-neovim`
- Downloads the compiled binary artifact
- Makes it executable
- Tests the binary by running `--version`

## Usage

### Automatic Builds
The workflow runs automatically every week via cron schedule. No action needed.

### Manual Builds
1. Go to the "Actions" tab in your GitHub repository
2. Select "Build Static Neovim" workflow
3. Click "Run workflow"

### Downloading the Binary
1. After a build completes, go to the workflow run
2. Download the `static-neovim` artifact
3. Extract and use the `nvim-static-x86_64` binary

## Building Locally

If you want to build locally, the process is similar:

```bash
# Install dependencies (Alpine Linux example)
apk add build-base cmake coreutils curl gettext-tiny-dev git ninja-build

# Clone and build
git clone --depth 1 --branch stable https://github.com/neovim/neovim
cd neovim
make CMAKE_EXTRA_FLAGS="-DSTATIC_BUILD=1"

# The binary will be at build/bin/nvim
```

## Binary Information

- **Architecture**: x86_64
- **Branch**: stable
- **Type**: Static binary (no external dependencies)
- **Size**: Typically ~15-20MB

## Documentation

For more detailed build instructions and options, see the official Neovim build documentation:
https://neovim.io/doc/build/

## License

This workflow follows the same license as Neovim. The built binary includes all Neovim components and their respective licenses.

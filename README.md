## Documentation Setup with MkDocs

### Prerequisites
```bash
# Install Python and pip (if not already installed)
# On macOS:
brew install python3

# Install MkDocs and Material theme
pip3 install mkdocs mkdocs-material
```

### MkDocs Configuration
Create `mkdocs.yml` in project root:
```yaml
site_name: Pi Cluster Setup Documentation
site_description: Comprehensive guide for Raspberry Pi cluster setup
site_author: Your Name

theme:
  name: material
  palette:
    primary: deep purple
    accent: purple
  features:
    - navigation.tabs
    - navigation.sections
    - navigation.expand
    - search.highlight

nav:
  - Home: index.md
  - Planning: planning.md
  - Hardware Setup: hardware.md
  - OS Setup: os-setup.md
  - Network Configuration: network.md
  - Kubernetes Setup: kubernetes.md
  - Troubleshooting: troubleshooting.md

markdown_extensions:
  - admonition
  - codehilite
  - toc:
      permalink: true
```

### Running MkDocs

#### Development Server
```bash
# Navigate to project root
cd /Users/jaeungjang/project/pi-cluster-setup

# Start development server (auto-reload on changes)
mkdocs serve

# Access documentation at: http://127.0.0.1:8000
```

#### Building Static Site
```bash
# Build static HTML files
mkdocs build

# Deploy to GitHub Pages (if configured)
mkdocs gh-deploy
```

#### Useful Commands
```bash
# Create new documentation project
mkdocs new my-project

# Validate configuration
mkdocs config

# Build with verbose output
mkdocs build --verbose

# Serve on custom port
mkdocs serve --dev-addr 127.0.0.1:8080
```

### Documentation Structure
```
```

### Writing Tips
- Use `!!! note` for important information
- Use `!!! warning` for critical warnings
- Include code blocks with syntax highlighting
- Add screenshots in `docs/assets/images/`
- Use relative links between pages
# AGENTS.md

AI development guidelines for `mconf-ci-jobs` - a reusable GitHub Actions workflow library.

## Project Structure

```
.github/workflows/  # Reusable workflow definitions
examples/           # Usage examples for each workflow
README.md           # Documentation index
```

## Workflow Naming Convention

- **Prefix**: Team name (`lb-`, `portal-`, etc.) or `all-` for language-agnostic workflows used across teams
- **Format**: `{prefix}-{language}-{action}.yml` or `{prefix}-{action}.yml`
- **AI suffix**: Use `-ai` suffix in filename and `(AI)` in action name when workflow uses AI
- **Examples**: `lb-go-build.yml`, `portal-ruby-tests.yml`, `all-gen-changelog-ai.yml`

## Development Principles

### Security Requirements
- **Minimal permissions**: Only grant what's strictly required (`contents: read`, `pull-requests: write`, etc.)
- **Timeout limits**: All jobs must have `timeout-minutes` (15-30 minutes typical)
- **Secret handling**: Never log secrets, use GitHub secrets for sensitive data
- **Self-hosted runners**: Target `[self-hosted, ubuntu-22.04]` unless otherwise needed

### Workflow Design Patterns
- **Reusability**: All workflows use `workflow_call` trigger
- **Versioning**: Consumers reference by branch/tag (`@main`, `@v1.0.0`)
- **Inputs**: Document all inputs with comments in example files
- **Runner configuration**: Include a `runs-on` input parameter (type: string, default: `'["self-hosted", "ubuntu-22.04"]'`) and use `${{ fromJSON(inputs.runs-on) }}` in job definitions to allow consumers to override the runner
- **Dependencies**: Minimize dependencies and reuse across workflows. If one workflow uses a dependency, favor reusing it in others
- **Dependency updates**: When updating a dependency in one workflow, review if others using it should be updated too
- **SSH support**: Include optional SSH agent setup when `SSH_PRIVATE_KEY` secret exists

### Code Quality Standards
- **DRY**: Extract common steps into composite actions if repeated across 3+ workflows
- **YAGNI**: Only add features when actually needed by consumers
- **Consistency**: Follow established patterns in existing workflows
- **Validation**: Test workflows run successfully on example repositories
- **Examples**: Every workflow must have a corresponding example file in `examples/` with documented inputs as comments
  - Format: `# parameter_name (required/optional): Description (default: value if applicable)`
  - Use inline comments on the same line as parameters
  - No blank lines between parameters
  - Show required parameters with actual values, optional parameters as commented examples

## Common Tasks

### Adding a New Workflow
1. Create workflow file in `.github/workflows/` with proper naming (use `-ai` suffix if using AI)
2. Define `workflow_call` trigger with required inputs
3. Set minimal permissions and timeout
4. Configure self-hosted runner target
5. Add dependency caching if applicable
6. Create example file in `examples/` with input documentation as comments
7. Document in README.md under appropriate section
8. Test on real repository before merging

### Modifying Existing Workflows
1. Check README for consumers that reference this workflow
2. Maintain backward compatibility or coordinate breaking changes
3. **Always review and update** the corresponding example file in `examples/`
4. If changing dependencies, check if other workflows use them and should be updated
5. Test changes don't break example usage

### Debugging Workflow Issues
- Check or request runner logs for actual error messages
- Verify secrets are configured in consuming repository
- Ensure permissions are sufficient for the actions performed
- Confirm dependencies (Go version, Node version, etc.) are correct

# Security Audit Checklist for Skills

Use this checklist when evaluating discovered skills during Phase 3 of the Forge workflow.

## Execution Safety

- [ ] **External script execution**: Does the skill run scripts from bundled `scripts/` directory? Are inputs to those scripts validated or sanitized?
- [ ] **Dynamic code execution**: Does the skill use `eval()`, `exec()`, `Function()`, or similar patterns? If so, is the input controlled?
- [ ] **Shell injection**: Does the skill construct shell commands from user input? Are arguments properly escaped/quoted?
- [ ] **Curl-pipe-bash**: Does the skill download and execute code from URLs? This is a critical risk pattern.

## Filesystem Access

- [ ] **Scope of access**: Does the skill read/write to specific directories, or does it access broad paths like `/`, `~`, or `$HOME`?
- [ ] **Sensitive directories**: Does it touch `~/.ssh/`, `~/.aws/`, `~/.config/`, `/etc/`, or other sensitive locations?
- [ ] **File overwrite risk**: Could the skill overwrite existing files without confirmation?
- [ ] **Temp file hygiene**: Does it clean up temporary files after use?

## Network & API

- [ ] **External API calls**: Does the skill call external APIs? Are endpoints hardcoded or user-configurable?
- [ ] **Credential handling**: Does it require or handle API keys, tokens, or passwords? How are they stored/passed?
- [ ] **Data exfiltration risk**: Could the skill send local file contents to external services?
- [ ] **Dependency downloads**: Does it install packages at runtime? From which registries?

## Prompt Injection Surface

- [ ] **Untrusted input processing**: Does the skill process content from files, web pages, or APIs that could contain adversarial instructions?
- [ ] **Instruction boundary clarity**: Are the skill's instructions clearly separated from user/external data?
- [ ] **Role confusion potential**: Could processed content trick Claude into treating data as instructions?

## Data Sensitivity

- [ ] **PII handling**: Does the skill process personally identifiable information?
- [ ] **Credential exposure**: Could credentials appear in logs, outputs, or error messages?
- [ ] **Output scope**: Are outputs saved to appropriate locations (not publicly accessible directories)?

## Severity Ratings

### Critical
Immediate risk to system security or data integrity. Examples:
- Downloading and executing unverified code
- Sending local files to external services without user consent
- Storing credentials in plaintext in accessible locations
- Unvalidated shell command construction from external input

### Warning
Potentially risky depending on usage context. Examples:
- Broad filesystem access that could be scoped more narrowly
- Optional API integrations without clear security guidance
- Package installation from public registries without version pinning
- Processing untrusted content without injection safeguards

### Info
Not inherently risky but worth noting for transparency. Examples:
- Installing well-known packages from standard registries
- Writing to temp directories with proper cleanup
- Reading from user-specified file paths (expected behavior)
- Using environment variables for configuration

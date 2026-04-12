# Contributing to Code Cannon

This guide covers setup and conventions for developing Code Cannon itself — not for using it in your projects.

## Development setup

### Pre-commit hook

Generated skill files (`.claude/commands/`, `.cursor/rules/`, `.agents/skills/`, `.gemini/skills/`) must stay in sync with source skills. A pre-commit hook enforces this.

Install it once per clone:

```bash
cat > .git/hooks/pre-commit << 'EOF'
#!/bin/sh
# Ensure agent skill files are in sync before committing.
# sync.sh --dry-run exits 1 if any files need regeneration.
./sync.sh --dry-run 2>/dev/null
if [ $? -ne 0 ]; then
  echo "Skills are out of sync. Run ./sync.sh --force before committing."
  exit 1
fi
EOF
chmod +x .git/hooks/pre-commit
```

If a commit is rejected, run `./sync.sh --force` and retry.

# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Environment

This project uses Dev Containers. All build/test/lint commands run inside the container.

## Commands

```bash
# Node.js
pnpm install          # 依存関係インストール
pnpm run build        # ビルド
pnpm run test         # テスト
pnpm run lint         # リント

# Python
pip install -r requirements.txt
pytest                # テスト
ruff check .          # リント
```

## Conventions

- TypeScript: strict mode, Conventional Commits
- Python: type hints 必須, ruff でフォーマット

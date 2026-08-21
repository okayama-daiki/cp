# AtCoder Notes

Quartz 5で構築した競技プログラミングのノートサイトです。サイト関連のファイルはこのディレクトリ内にまとめています。

```bash
bun install --frozen-lockfile
bun run dev
```

生成されたサイトは`public/`に出力されます。
型チェックとBiomeのLint・整形チェックは`bun run check`、自動修正は`bun run format`で実行できます。

各記事は`content/notes/<記事名>/index.md`に置き、記事固有の画像は`index.md`と同じディレクトリに置きます。画像は`![代替テキスト](./image.png)`のように参照してください。

## GitHub Pages

`main`ブランチへpushすると、`.github/workflows/deploy-pages.yml`がBunでサイトをビルドしてGitHub Pagesへ自動公開します。

公開URLは https://okayama-daiki.github.io/cp/ です。

# doc-portfolishowcaseo-app

## 【Next.js + GitHub API 入門】初心者向けハンズオン勉強会【① / 全2回】

## スケジュール

## 本日のお題

ショーケースアプリ（GitHub リポジトリのまとめサイト）

レベル：中級

このプロジェクトでは、GitHub リポジトリのショーケースアプリを構築します。

個人のポートフォリオから、特定の技術に基づいたまとめサイトまで、
多岐にわたって、組み込まれることが多い、一般的なショーケースです！

GitHub API を使用して、リポジトリのデータを取得し、一覧と詳細を表示します。

## 学習する内容

* Next.js v15/ App Router の使用
* TypeScriptによる型チェック
* shadcn/ui によるデザインの導入
* GitHub API の使用
* md ファイルの取り扱い
* supabase を使用したデータベース連携

この勉強会を通して、React を使った成果物を１つ構築することを目指します！

## ハンズオンを進めるための前提条件

Node.js 環境（v18.18.0 以上）が構築されたPCが必要です
Git/ GitHub環境（アカウント）の用意
supabase のアカウントが必要です
HTML/CSS/JavaScript の基礎知識が必要です
React の基礎（Progateレベルの内容）は理解している前提の内容となっております

## ページ構成と機能仕様

以下のユーザーストーリーを満たすことを、確認してください：

TOP ページ：
 ユーザーがサイトにアクセスすると、コンテンツ一覧を閲覧できる
 トピックのボタンを押すと、一致するコンテンツのみが表示される
 個別のコンテンツをクリックして、詳細ページに遷移できる
コンテンツ詳細ページ:
 ユーザーは各コンテンツの詳細情報（README.md）を確認できる
 GitHub と同じ、「Domain.com/オーナー名/リポジトリ名」という URL の構造になっている
 外部リンクへのボタンをクリックして、GItHub のリポジトリ・デモサイトに遷移できる
 コンテンツに対する、リアクションの総数を確認することができる
 誰でもログインせずに、コンテンツに対して絵文字でリアクションすることができる
 誰でもログインせずに、コンテンツに対するリアクションを削除することができる
 「いいね」数の複数インクリメントなどの不正操作は、許容する方針で、済ませること

## 開発手順

環境構築
Next.js の導入（TS+Tailwind）
shadcn/ui の導入
サイトレイアウトの作成
GitHub API からのデータフェッチロジックの追加
コンテンツ一覧の実装
コンテンツのフィルタリング機能の追加
詳細ページの実装(md ファイルの扱い)
supabase の導入
リアクションコンポーネントの作成
リアクション機能の実装(server actions)

## Chapter 01 Next.js の導入

### 1. このチャクターの目次

- [Next.js / App Router とは？](#nextjs--app-router-とは)
- [Next.js の導入手順](#nextjs-の導入手順)
- [FAQ](#faq)
- [まとめ](#まとめ)
- [おまけ：git commit](#おまけgit-commit)

---

### 1, Next.jsの主な特徴

Next.js は、**React ベースのモダンな Web アプリケーションフレームワーク**です。  
主な特徴をいくつか紹介します。

- **サーバーサイドレンダリング（SSR）**
  - ページをサーバー側で事前にレンダリングし、初期表示を高速化  
  - SEO にも有利です
  - 必要に応じて静的サイト生成（SSG）や増分静的再生成（ISR）も可能です

- **App Router**
  - Pages Router より新しいルーティングシステム  
  - ディレクトリベースのルーティング  
  - サーバーコンポーネントとクライアントコンポーネントを組み合わせ、高速で柔軟な開発が可能です

- **その他の特徴**
  - 自動コード分割：ページごとに必要な JavaScript のみ読み込み  
  - データキャッシュ：リクエストのメモ化・再検証が容易  
  - サーバーアクション：ユーザー操作に応じた非同期処理をサーバー上で実行可能  

---

### 2. Next.js の導入手順

以下の手順に沿って、Next.js 環境を構築していきます。  
公式ドキュメントも参考にしながら進めてください。

**1. Node.js のインストール確認**

Next.js には **Node.js v18.18.0 以上** が必要です。  
以下のコマンドでバージョンを確認してください。

```bash
%  node -v
```
v22.18.0

**2. プロジェクトディレクトリを作成**

ターミナルから、お好きな場所にプロジェクト用ディレクトリを作成して移動します。

```bash
mkdir portfolio-showcase-app
```

```bash
cd portfolio-showcase-app
```

---

**3. Next.js プロジェクトを作成**

Next.js では、自動設定コマンドを使用して簡単にプロジェクトを生成できます。

```bash
npx create-next-app@latest
```

プロンプトが表示されたら、以下のように入力・選択してください。

「次のパッケージ（create-next-app@15.5.6）を一時的にインストールする必要があります。
進めてもよいですか？」

Need to install the following packages:
create-next-app@15.5.6
Ok to proceed? (y) 

指定のバージョンのcreate-next-appがインストールされている場合は表示されません。

そのまま y を入力して Enterキー を押します。

`? What is your project named? › .` ビリオドを入力してエンターをクリック
`? Would you like to use TypeScript? › Yes`Yesが選択されている状態でエンターキーをクリック
`? Which linter would you like to use? › - Use arrow-keys. Return to submit.
❯   ESLint`ESLintが選択されている状態でエンターキーをクリックする
`? Would you like to use Tailwind CSS? › Yes` Yesが選択されている状態でエンターキーをクリック
`? Would you like your code inside a `src/` directory? › Yes`Yesが選択されている状態でエンターキーをクリック
`? Would you like to use App Router? (recommended) › Yes`Yesが選択されている状態でエンターキーをクリック
`? Would you like to use Turbopack? (recommended) › Yes`Yesが選択されている状態でエンターキーをクリック
`? Would you like to customize the import alias (`@/*` by default)? › No `Noが選択されている状態でエンターキーをクリック

Success! Created portfolio-showcase-app at /Users/アカウント名/portfolio-showcase-app

---

### 3. 開発サーバーを起動

作成が完了したら、開発サーバーを起動して動作を確認します。

```bash
npm run dev
```

ブラウザで [http://localhost:3000](http://localhost:3000) を開き、Next.js の初期画面が表示されれば成功です。

---

### 4. gitでプロジェクトを管理する

プロジェクトの進捗を追跡し、作業履歴を残すために、タスクごとにコミットを行うことは非常に重要です。

Next.jsでは、create-next-app 実行後は、最初のコミットが自動で追加されますので、このタスクでは新たにコミットする必要はありません。
以降の開発では、タスクが一区切りつくごとに以下のようにコミットしていきましょう。

---

### 5. 本章のまとめ

**Turbopack とは？**

Turbopack は、次世代のバンドラーです。

**高速な開発サーバー**

npm run dev（開発モード）で使用され、
コードの変更が即座にブラウザへ反映されます。

従来の Webpack よりも数十倍高速に動作します。

**増分ビルド**

ファイル全体を再ビルドするのではなく、変更のあった部分のみを再コンパイル。

**バンドラー（Bundler）とは？**

バンドラーとは、以下の機能があります。

1. 複数のファイル（JavaScript、CSS、画像など）を 1つまたは少数のファイルにまとめるツール のことです。
2. あるファイルが別nファイルw必要としている場合、自動で正しい順番に真tメル
3. 最適化（圧縮・トランスパイル）
   * 不要なコードの削除や、古いブラウザ向けに交換
  
**ホットリロード**
コードの変更内容が即座に反映されます。

このチャプターでは、次の内容を学びました。

1. create-next-app コマンドを使って、簡単に Next.js プロジェクトをセットアップできる
2. Turbopack によって、開発サーバーの動作が非常に高速化されている
3. Node.js の環境確認から開発サーバーの起動までの流れを理解した


## Chapter 02 shadcn/ui の導入
## Chapter 03 サイトレイアウトの作成
## Chapter 04 GitHub API からのデータフェッチロジックの追加
## Chapter 05 コンテンツ一覧の実装
## Chapter 06 コンテンツのフィルタリング機能の追加
## Chapter 07 詳細ページの実装(md ファイルの扱い)
## Chapter 08 supabase の導入
## Chapter 09 リアクションコンポーネントの作成
## Chapter 10 リアクション機能の実装(server actions)



# 構築手順
## 1. Solana CLIのインストール
1. CLIのインストール
   ```bash
   sh -c "$(curl -sSfL https://release.solana.com/stable/install)"
   ```
2. 追加するPATHを控える
   ```bash
   Close and reopen your terminal to apply the PATH changes or run the following in your existing shell:
   export PATH="/Users/XXXX/.local/share/solana/install/active_release/bin:$PATH"
   ```
3. PATHの設定
   ```bash
   echo $SHELL
   #
   vim ~/.zshrc
   source ~/.zshrc
   ```
4. インストールされたか確認（バージョンの出力）
   ```bash
   solana --version
   ```

## 2. テスト用バリデータの立ち上げ
1. 立ち上げのコマンド実行
   ```bash
   solana-test-validator
   ```
2. X

## 2.5 バリデータ立ち上げでハマったので、以下コードにより修復
Githubから諸々のライブラリをインストールしているよう
```bash
git clone --depth 1 --branch v1.14.3 https://github.com/solana-labs/solana.git
cd solana
./scripts/cargo-install-all.sh .
echo "export PATH=$PWD/bin:\$PATH" >> ~/.profile
```

## 3. テストネットへの接続
1. テストネットへ設定を変更
   ```bash
   solana config set --url devnet
   ```
2. FaucetからSOLをエアドロップ
   ```bash
   solana airdrop 2  
   ```
3. X

## 4. コントラクトの記述〜ビルド1〜デプロイ
1. Cargoプロジェクト立ち上げ（初期化）
   ```bash
   cargo init hello_world --lib
   cd hello_world
   ```
2. ビルド
   ```bash
   anchor build
   anchor deploy --provider.cluster devnet
   ```
3. X

## 5. Webアプリからの実行
1. サンプルコードのCLone
2. 環境変数の設定
   ### IDLの変更
   * コピー元：[counter/target/idl/counter.json](./counter/target/idl/counter.json)
   * コピー先：[sample_webapp/src/idl.json](./sample_webapp/src/idl.json)の内容を変更

   ### プログラムIDの変更
   [anchorClient.ts](./sample_webapp/src/anchorClient.ts)の９行目を変更
   ```rust
   const programId_counter = new PublicKey( -->
      "9nQUPGZ32Y3D71tZAoUPJGERQAiZZoDmdmNPXCLyRyBr"
   );
   ```
3. X

# 環境値
項目 | 値
-- | --
Program ID | 9nQUPGZ32Y3D71tZAoUPJGERQAiZZoDmdmNPXCLyRyBr

# 参考
* [【完全保存版】ローカル環境でのsolana CLIを実行（コントラクトのデプロイまで）](https://note.com/standenglish/n/nd90e38db1781)
* [solana-test-validator: illegal hardware instruction](https://solana.stackexchange.com/questions/3640/solana-test-validator-illegal-hardware-instruction)
* [Setup, build, and deploy a Solana program locally in Rust](https://solana.com/developers/guides/getstarted/local-rust-hello-world)
* [Default program won't build on Anchor 0.30, ahash 0.84 no longer builds](https://solana.stackexchange.com/questions/13210/default-program-wont-build-on-anchor-0-30-ahash-0-84-no-longer-builds)
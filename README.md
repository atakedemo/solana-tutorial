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

## 4. コントラクトの記述〜ビルド
1. Cargoプロジェクト立ち上げ（初期化）
   ```bash
   cargo init hello_world --lib
   cd hello_world
   ```
2. Solanaライブラリのインストール ※バージョン指定
   ```bash
   cargo add solana-program@"=1.17.17"
   ```
3. Cargo .tomlに以下記述を追加
   ```bash
   [lib]
   name = "hello_world"
   crate-type = ["cdylib", "lib"]
   ```
4. 処理コードにコントラクト固有の情報を記述
   ```rust
   // 1.Solanaのライブラリ系をインポート
   use solana_program::{
      account_info::AccountInfo,
      entrypoint,
      entrypoint::ProgramResult,
      pubkey::Pubkey,
      msg,
   };

   // 2. Entrypointの指定
   entrypoint!(process_instruction);
   pub fn process_instruction(
      program_id: &Pubkey,
      accounts: &[AccountInfo],
      instruction_data: &[u8]
   ) -> ProgramResult {
      // log a message to the blockchain
      msg!("Hello, world!");
   
      // gracefully exit the program
      Ok(())
   }

   pub fn add(left: usize, right: usize) -> usize {
      left + right
   }
   #[cfg(test)]
   mod tests {
      use super::*;

      #[test]
      fn it_works() {
         let result = add(2, 2);
         assert_eq!(result, 4);
      }
   }
   ```
5. プログラムをビルド(Cago.tomlと同ディレクトリで実行)
   ```bash
   cargo build-bpf
   ```
## 5. コントラクトのデプロイ
1. デプロイの実行
   ```bash
   solana program deploy ./target/deploy/hello_world.so
   ```
2. プログラムのコードを控えておく（以下は出力値の例）
   ```bash
   Program Id: EFH95fWg49vkFNbAdw9vy75tM7sWZ2hQbTTUmuACGip3
   ```

# 環境値
項目 | 値
-- | --
Program ID | 6hvQdryW4ic8so5h11FZ1QvNFszhoc38guByakbdLvaL

# 参考
* [【完全保存版】ローカル環境でのsolana CLIを実行（コントラクトのデプロイまで）](https://note.com/standenglish/n/nd90e38db1781)
* [solana-test-validator: illegal hardware instruction](https://solana.stackexchange.com/questions/3640/solana-test-validator-illegal-hardware-instruction)
* [Setup, build, and deploy a Solana program locally in Rust](https://solana.com/developers/guides/getstarted/local-rust-hello-world)
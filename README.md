# git利用コマンド一覧

## 基本的な物

- ローカル変更部分の抽出
   
  ```bash
  git status
  ```

  ```bash
  git diff
  ```
- ステージング
  ```bash
  git add 対象ファイル
  ```
- コミット
  ```bash
  git commit -m コミットコメント
  ```
- リモートpush
  ```bash
  git push
  ```

- リモートpull
  ```bash
  git pull
  ```
- 一時退避
  ```bash
  git stash 
  ```
- 一時退避(メッセージ付き)
  ```bash
  git stash save "〇〇作業中のファイル"
  ```
- 一時退避の一覧
  ```bash
  git stash list
  ```
- 退避物の復活
  ```bash
  git stash pop
  ```
- 特定の者の復活(git stash list の番号を指定する)
  ```bash
  git stash pop stash@{1}
  ```
- ブランチマージ

  - ブランチ先に切り替え
    ```bash
    git checkout main
    ```
  - ブランチ対象をマージ
    ```bash
    git merge ブランチ
    ```
    衝突(conflict)が発生した場合、次のメッセージが表示される
    
    ```
    You have unmerged paths.
      (fix conflicts and run "git commit")
      (use "git merge --abort" to abort the merge)

    Unmerged paths:
      (use "git add <file>..." to mark resolution)
            both modified:   main.txt

    no changes added to commit (use "git add" and/or "git commit -a")
    ```

    こういうメッセージの場合もある
    ```
    Auto-merging main.txt
    CONFLICT (content): Merge conflict in main.txt
    Automatic merge failed; fix conflicts and then commit the result.
    ```        
    
    対象のファイルは、次のように各ブランチ毎の内容が=======区切りで記載されているので、適切な内容に変更して、git add、git commit にて改修する。  

    
    cat main.txt
    ```
    mainmodify
    sub1modify
    A=C
    B=X
    A=xxxxx
    <<<<<<< HEAD
    branch=mainブランチの変更部分です
    
    =======
    branch=sub1ブランチの変更です
    >>>>>>> sub1
    ```


## ブランチ関連

- ローカルブランチ
  - ブランチの一覧
    ```bash
    git branch
    ```
  - ローカルブランチの作成
    ```bash
    git branch ブランチ名
    ```
  - ブランチの変更
    ```bash
    git checkout ブランチ名
    ```

- リモートブランチ
  - リモートブランチの一覧
    ```bash
    git fetch
    git branch -a
    ```
  - リモートブランチの取り込み
    ```bash
    git switch リモートブランチ名(remotes/origin/部分を取り除いた名前)
    ```
  - ローカルブランチをリモート登録
    ```bash
    git push -u origin ブランチ名
    ```
    
## 変更部分抽出、適用関連

- 変更部分のpatch作成(変更部分の確認にも使える)
  - 修正作業ツリーの修正(ステージングしていない変更)のpatch作成
    ```bash
    git diff > wip.patch
    ```
  - patchを適用(作業ツリーに反映)
    ```bash
    git apply パッチファイル
    ```
  - patchを適用と同時にステージング
    ```bash
    git apply --index パッチファイル
    ```
  - ベース違いでもmerge的に当てる
    ```bash
    git apply --3way パッチファイル
    ```
  - patch前に戻す
    ```bash
    git apply -R パッチファイル
    ```
  - コミットidを指定してpatch作成(コミットコメント込み)
  　```bash
    git format-patch -1 commit-id --stdout --binary > パッチファイル
    ```
  - 取り出したpatchをcommit コメント込みで適用
    ```bash
    git am パッチ
    ```
  
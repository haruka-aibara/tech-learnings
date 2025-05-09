# Git resetコマンド

## 概要
`git reset`はコミット履歴を操作し、特定の状態に戻すための強力なコマンドです。

## 基本概念
`git reset`は主に「HEADの位置」「インデックス（ステージングエリア）」「ワーキングディレクトリ」の3つの状態を変更するために使用されます。

## モード
`git reset`には3つの主要なモードがあります：

1. **--soft**: HEADの位置のみを変更します
   ```
   git reset --soft <commit>
   ```
   - 変更内容はステージングエリアに残ります
   - 直前のコミットをやり直したい場合に便利です

2. **--mixed** (デフォルト): HEADの位置とインデックスを変更します
   ```
   git reset <commit>
   ```
   または
   ```
   git reset --mixed <commit>
   ```
   - 変更内容はワーキングディレクトリに残ります（ステージングが解除されます）
   - コミットとステージングをやり直したい場合に便利です

3. **--hard**: HEAD、インデックス、ワーキングディレクトリすべてを変更します
   ```
   git reset --hard <commit>
   ```
   - 変更内容はすべて失われます（注意！）
   - 完全に特定のコミット状態に戻したい場合に使用します

## 一般的な使用例

### 直前のコミットを取り消す
```
git reset --soft HEAD~1
```
- 直前のコミットを取り消しますが、変更内容はステージングエリアに残ります

### ステージングした変更を取り消す
```
git reset HEAD <file>
```
または
```
git restore --staged <file>
```
- 特定のファイルのステージングを解除します（Git 2.23以降は`git restore`を推奨）

### 特定のコミットまで履歴を巻き戻す
```
git reset --hard <commit-hash>
```
- 指定したコミットの状態にすべてを戻します（注意！変更は失われます）

## 注意点

1. **--hardオプションは慎重に**: 未コミットの変更がすべて失われます

2. **公開リポジトリでの使用は避ける**: `git reset`は履歴を書き換えるため、すでにプッシュした変更に対しては使用を避けましょう（代わりに`git revert`を検討）

3. **REFLOGを活用**: 万が一の場合、`git reflog`を使用して操作を元に戻せる場合があります
   ```
   git reflog
   git reset --hard HEAD@{1}  # 一つ前の操作に戻る
   ```

## git resetとgit revertの違い
- `git reset`: 履歴を書き換え、コミットを削除します
- `git revert`: 新しいコミットを作成して変更を打ち消します（履歴は保持）

## まとめ
`git reset`コマンドは状態を巻き戻すための強力なツールですが、特に`--hard`オプションを使用する場合は細心の注意が必要です。基本的に個人の作業ブランチでのみ使用し、共有リポジトリでは`git revert`を優先することをお勧めします。

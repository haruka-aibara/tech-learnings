# ファイル読み込み・書き込み操作

## 概要
ファイル操作はプログラミングの基本的かつ重要なスキルであり、データの永続化や共有に不可欠です。

## 主要概念
Pythonでは、open()関数とさまざまなモード（'r'読み込み、'w'書き込み、'a'追加、'+'更新）を使用してファイルを操作します。

## 実践例

### 1. テキストファイルの作成

以下のコードを入力して実行してみましょう：

```python
# 複数行の文字列を定義
content = """\
AAA
BBB
CCC
DDD
"""

# ファイルを書き込みモードで開き、内容を書き込む
with open('test.txt', 'w') as f:
    f.write(content)
```

**実行結果**：
ファイル「test.txt」が作成され、指定した内容が書き込まれます。ファイルエクスプローラーで確認してみましょう。

### 2. ファイルの全内容を一度に読み込む

以下のコードを入力して実行してみましょう：

```python
# ファイルを読み込みモードで開き、すべての内容を一度に読み込む
with open('test.txt', 'r') as f:
    print(f.read())
```

**実行結果**：
```
AAA
BBB
CCC
DDD
```

**解説**：
`f.read()`メソッドはファイルの内容をすべて一つの文字列として読み込みます。

### 3. ファイルを1行ずつ読み込む

以下のコードを入力して実行してみましょう：

```python
# ファイルを1行ずつ読み込む
with open('test.txt', 'r') as f:
    while True:
        line = f.readline()  # 1行読み込み
        print(line, end='')  # 改行を追加しないように end='' を指定
        if not line:         # 空文字列になったら（ファイル終端）ループを抜ける
            break
```

**実行結果**：
```
AAA
BBB
CCC
DDD
```

**解説**：
`readline()`メソッドは1行ずつ読み込みます。ファイルの終わりに達すると空文字列が返されます。

### 4. チャンク（固定サイズ）ごとに読み込む

以下のコードを入力して実行してみましょう：

```python
# ファイルを指定したバイト数ずつ読み込む
with open('test.txt', 'r') as f:
    while True:
        chunk_size = 2       # 一度に読み込む文字数
        chunk = f.read(chunk_size)  # 指定サイズ分読み込み
        print(chunk)
        if not chunk:        # 空文字列になったら（ファイル終端）ループを抜ける
            break
```

**実行結果**：
```
AA
A

BB
B

CC
C

DD
D


```

**解説**：
`f.read(chunk_size)`は指定した文字数だけ読み込みます。改行文字も1文字としてカウントされるため、出力結果が2文字ずつに分割されます。

### 5. ファイル内の位置を操作する（seek）

以下のコードを入力して実行してみましょう：

```python
# seekを使ってファイル内の位置を移動する
with open('test.txt', 'r') as f:
    print(f.tell())        # 現在の位置を表示（開始位置は0）
    print(f.read(1))       # 1文字読み込み（位置0の文字='A'）
    f.seek(5)              # 5バイト目に移動
    print(f.read(1))       # 1文字読み込み（位置5の文字）
    f.seek(14)             # 14バイト目に移動
    print(f.read(1))       # 1文字読み込み（位置14の文字）
    f.seek(15)             # 15バイト目に移動
    print(f.read(1))       # 1文字読み込み（位置15の文字）
    f.seek(5)              # 再び5バイト目に移動
    print(f.read(1))       # 1文字読み込み（位置5の文字）
```

**実行結果**：
```
0
A
B
D
D
B
```

**解説**：
- `tell()`はファイル内の現在位置を返します
- `seek(n)`でファイル内のn番目の位置に移動します
- Windows環境では改行が`\r\n`で2バイトなので、位置が異なる場合があります

### 6. 書き込み読み込みモード（w+）

以下のコードを入力して実行してみましょう：

```python
# 書き込み読み込みモード（w+）の利用
content = """\
AAA
BBB
CCC
DDD
"""

with open('test.txt', 'w+') as f:  # w+: 書き込み+読み込みモード（ファイルを空にしてから開く）
    f.write(content)             # 内容を書き込み
    f.seek(0)                    # ファイルポインタを先頭に戻す
    print(f.read())              # 内容を読み込んで表示
```

**実行結果**：
```
AAA
BBB
CCC
DDD
```

**解説**：
`w+`モードはファイルを書き込み+読み込みモードで開きます。ファイルが存在する場合は内容を空にしてから開き、存在しない場合は新規作成します。書き込み後に読み込むには、`seek(0)`でファイルポインタを先頭に戻す必要があります。

### 7. 空のファイルを書き込み読み込みモードで開く

以下のコードを入力して実行してみましょう：

```python
# 書き込み読み込みモード（w+）でファイルを開くとファイルは空になる
with open('test.txt', 'w+') as f:
    print(f.read())  # ファイルは空なので何も表示されない
```

**実行結果**：
```
```

**解説**：
`w+`モードでファイルを開くと、ファイルの内容は空になります。そのため、何も書き込まずに読み込むと、何も表示されません。

### 8. 存在しないファイルを読み込み書き込みモード（r+）で開く

以下のコードを入力して実行すると**エラーが発生します**：

```python
# 読み込み書き込みモード（r+）で存在しないファイルを開くとエラーになる
try:
    with open('test2.txt', 'r+') as f:  # r+: 読み込み+書き込みモード（ファイルが存在する必要がある）
        print(f.read())
        f.seek(0)
        f.write(content)
except FileNotFoundError:
    print("Error: test2.txt ファイルが見つかりません")
```

**実行結果**：
```
Error: test2.txt ファイルが見つかりません
```

**解説**：
`r+`モードはファイルが既に存在することを前提としています。存在しないファイルを`r+`モードで開こうとすると、`FileNotFoundError`が発生します。

### 9. 読み込み書き込みモード（r+）の正しい使い方

以下のコードを入力して実行してみましょう：

```python
content = """\
AAA
BBB
CCC
DDD
"""

# まずファイルを作成
with open('test.txt', 'w+') as f:
    f.write(content)

# 次に読み込み書き込みモード（r+）で開く
with open('test.txt', 'r+') as f:
    print(f.read())       # ファイルの内容を読み込み
    f.seek(0)             # ファイルポインタを先頭に戻す
    f.write(content)      # 内容を書き込み（上書き）
```

**実行結果**：
```
AAA
BBB
CCC
DDD
```

**解説**：
`r+`モードは既存のファイルを読み込みと書き込みの両方のモードで開きます。ファイルの内容は保持されるため、まず内容を読み込み、その後`seek(0)`でファイルポインタを先頭に戻してから書き込むと、内容を上書きすることができます。

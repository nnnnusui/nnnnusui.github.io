# Clean Architecture 解釈メモ
## 背景
検索ワードとして用いたのみで、書籍は読んでいない。  
時折りプロジェクトに適用しようとしてみるなどして消化中。

## 現状の認識
外部とデータをやり取りする際のノウハウとして認識。  
結局のところこう分けると良さげだな となったのはこんな形。

```
Entity: 閉じたロジックとデータ
UseCase(Service): やり取りのinterface
┗ Input, Output, TableRow: [入力, 出力, 保存] 時のデータ定義
Repository: UseCaseを跨いで保持するデータの切り分け
Controller, Presenter: 入力, 出力
```

こんなデータの流れになる。
```
Controller -(Input)-> UseCase
UseCase { Input => Entity => TableRow } <-(TableRow)-> Repository
UseCase { ((Input, Entity, )TableRow) => Output }
UseCase -(Output)-> Presenter
```

### 補足
4つもあるデータの依存関係は概ね  
```
Entity => [TableRow => Output, Input]
```
で、しかしシンプルなREST APIなんかに対しては冗長すぎるので
```
TableRow  => [OutPut, Input]
```
かな、という感じ。

## 結論
綺麗なコードになり得る。しかし書くのが面倒すぎる。  
要件が複雑になると分けた意味が出てくるが、データの被りは通常大分あるだろうから同じことを複数箇所に書くことにはほぼ絶対なる。  
TypeScriptにおけるDiffとかExtract的なものを使って横着したくなる。したい。いつかしてみて、どんな問題(要求)が出るか確認してみよう…。

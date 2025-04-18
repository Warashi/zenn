---
title: "fmt.Errorfのメッセージをチェックする wrapmsg という linter を書きました"
emoji: "🐕"
type: "tech"
topics:
  - "go"
  - "golang"
published: true
published_at: "2022-02-18 09:14"
---

fmt.Errorf でエラーを wrap するとき、メッセージはどんなことを書いていますか？
うちのチームではここにコード規約があり、そのエラーを返した関数やメソッドの呼び出し時の記述をそのまま記載することになっています。たとえば以下みたいな感じです。

```go
func hoge() error {
	var a A
	if err := a.B.C().D(); err != nil {
		return fmt.Errorf("a.B.C.D: %w", err)
	}
	return nil
}
```

コード規約があるということは、それに従っていないPRのレビュー時には指摘するべきということです。とはいえ人が見ているのではどうしても漏れがでます。
そこで linter を書きました。
<https://github.com/Warashi/wrapmsg> になります。
`go vet` に vettool として渡すこともできますし、 singlecheckerやmulticheckerを用いて単体のバイナリをビルドすることもできます。
SuggestedFixを実装してあるので、singlecheckerやmulticheckerでビルドした場合には `--fix` オプションで自動修正できたりもします。

fmt.Errofのメッセージを毎回考えて悩んでいる人、ぜひ使ってみてください。

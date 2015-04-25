# "textInput" バインディング

### 目的 {#purpose}

`textInput` バインディングはテキストボックス( `<input>` ) または テキストエリア( `<textarea>` ) をビューモデルのプロパティとリンクし、ビューモデルのプロパティと要素の値の間に双方向の更新を提供します。 `value` バインディングとは異なり、 `textInput` は オートコンプリート、ドラッグアンドドロップ、クリップボードイベントを含む全ての種類のユーザー入力に対してDOMを即座に更新します。

### 例 {#example}

```html
<p>Login name: <input data-bind="textInput: userName" /></p>
<p>Password: <input type="password" data-bind="textInput: userPassword" /></p>

ViewModel:
<pre data-bind="text: ko.toJSON($root, null, 2)"></pre>

<script>
    ko.applyBindings({
        userName: ko.observable(""),        // Initially blank
        userPassword: ko.observable("abc")  // Prepopulate
    });
</script>
```

### パラメータ {#parameters}

* メインパラメータ

  KO は要素のテキスト内容をパラメータの値に設定します。それまでの全ての値は上書きされます。

  もし、パラメータがobservable 値の場合、このバインディングはobservable の値の変更に応じて要素の値を更新します。パラメータがobservable値ではない場合、要素の値は一度のみ設定され、再びそれが更新されることはありません。

  もし、あなたが数値または文字列以外の何かを供給した場合 (例えば、object や配列を渡すなど)、表示されるテキストは `yourParameter.toString()` と同等になるでしょう (これは通常あまり有用ではないので、文字列または数値の値を供給するのがベストです)。

  ユーザが関連するフォームコントロールの値を編集するたびに、KO はビューモデル上のプロパティを更新します。KO は、ユーザまたは何らかのDOMイベントが値を変更した時、常にビューモデルを更新しようと試みます。

* 追加パラメータ
  * なし

### 注1: `textInput` と `value` バインディング {#note-1-textinput-vs-value-binding}

[`value` バインディング](./value-binding) もまた、 テキストボックスとビューモデルのプロパティ間で双方向のバインディングを行いますが、あなたが即時アップデートを望む場合、 `textInput` を使用するべきです。主な違いは:

＊即時アップデート

`value` はデフォルトでは、ユーザがテキストボックスからフォーカスアウトした時のみ、あなたのモデルを更新します。 `textInput` は各キー入力または他のテキスト入力の機構により、即座にあなたのモデルを更新します(テキストの切り取りやドラッグなど、これらは何らかのフォーカス変更イベントの発火を必要としません)。

＊ブラウザイベントの癖のハンドリング

各ブラウザは、切り取り、ドラッグまたはオートコンプリート候補の受け入れなど、普通ではないテキスト入力の機構に対するイベントの発火について、かなり一貫性がありません。 `value` バインディングは 特定のイベントによる更新のために `valueUpdate: afoterkeydown` のような拡張オプションを持ちますが、全ブラウザの全てのテキスト入力シナリオに対応しているわけではありません。

`textInput` バインディングは 特に幅広いブラウザの癖をハンドリングするためにデザインされ、普通ではないテキスト入力の機構に対しても、一貫性のある即時のモデル更新を提供します。

同じ要素に対して `value` と `textInput` バインディングの両方を使用しないでください。それは全く有益ではありません。

### 依存関係 {#dependencies}

なし、Knockout コアライブラリのみ。
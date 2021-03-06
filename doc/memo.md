- 単一ファイルコンポーネントのscriptブロックにおいては、exportが必須。
- コンポーネントにデータを持たせる場合、dataオプションは関数にしてreturnする必要がある。各インスタンスが返されるデータオブジェクトの独立したコピーを保持できるため。（[参考1]([https://jp.vuejs.org/v2/guide/components.html#data-%E3%81%AF%E9%96%A2%E6%95%B0%E3%81%A7%E3%81%AA%E3%81%91%E3%82%8C%E3%81%B0%E3%81%AA%E3%82%8A%E3%81%BE%E3%81%9B%E3%82%93](https://jp.vuejs.org/v2/guide/components.html#data-は関数でなければなりません)), [参考2](https://qiita.com/shosho/items/115b371736207078289c)）

```javascript
<script>
export default {
  data() {
    return {
      siteName: "",
      siteURL: ""
    };
```



### Vue Loader

- 単一ファイルコンポーネントをJavaScriptに変換するwebpackのloader
  - webpack: JavaScriptファイルをまとめる高機能なモジュールバンドラー


### moment.js

- moment.jsとそれをVueのfilter向けにラップしたvue-moment.jsのどちらを使うかで悩んだ。filterでmoment.js使いたいというより、全般的にmoment.js使いたかったから、とりあえずmoment.jsを使うことにした。vue-moment.jsはあまり情報もなかったので。
- moment.jsを使った上で、new Date()のところでmoment.js呼び出してフォーマットしてしまうのではなく、new Date()のところはそのままにしておいて、後で呼び出しの際にfilterで処理するようにした。レンダリングに時間はかかりそうだが、別の箇所でフォーマット変えるなど、応用が利きそうなので。

### アロー関数

- アロー関数は基本的に無名関数の省略形
- 無名関数？

```javascript
var hoge = function hoge(){...};	// 通常の関数
var hoge = function (){...};			// 無名関数。関数名決めなくてよくなる
var hoge = () => {...};						// アロー関数。functionの代わり
```

### フィルタの参考サイト

- [Vue Filter And Search | Vuetify](https://codesandbox.io/s/536mvppkq4)
- [Vuetify公式Data tables](https://vuetifyjs.com/ja/components/data-tables)

### moment.jsを使った時刻取得

- 上記「moment.js」で述べたとおり、filterでフォーマット変えるのやろうとしたが、data tableの項目にどうやってfilterをかませるかわからなかった。  
  そこで暫定対処として、filterじゃなくてAddSite.vueで登録する時点でフォーマット決めて登録してしまおうとした。  
  そしたら以下の警告が出て、ちょっとやった感じ警告でなくする方法がわからなかった。  
  なので暫定対処頑張るのではなく、data tableの項目にfilterかます方向に倒そうと思う。そのうちやる。

Deprecation warning: value provided is not in a recognized RFC2822 or ISO format. moment construction falls back to js Date(), which is not reliable across all browsers and versions. Non RFC2822/ISO date formats are discouraged and will be removed in an upcoming major release. Please refer to http://momentjs.com/guides/#/warnings/js-date/ for more info.

### 時刻フォーマット機能の実装

[Vuetify.jsのData tablesでフィルタリングを行う](https://qiita.com/d-yosh/items/9299389b32c496a9b64c)を確認したところ、あるカラムに対してフィルタをかける場合にはcustom-filterを使うのが良さそう。ちなみにちょっと試した感じ、複数のフィルタ（custom-filterではなく普通のフィルタ）をかける方法がわからなかった。具体的には、:filter="filter" search="大阪府"の記述を既存のコードに入れたところ、searchは２個指定できないと怒られた。search="大阪府"のsearchを別の文字列にするとこのフィルタが動かなくなってしまった。searchは１つで済むよう関数側でうまくやる必要がある？今回はcustom-filterを使うから関係なさそうだが、よくわからなかったポイントとして記録しておく。

```html
<v-data-table :headers="headers" :items="sites" :search="search" :filter="filter" search="大阪府">
```


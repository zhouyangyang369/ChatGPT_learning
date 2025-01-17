### 参考文献：https://qiita.com/ksonoda/items/b767cbd283e379303178

# 学習データ
![image](https://github.com/user-attachments/assets/5a02effd-cf23-45f2-abf6-74ccc156befa)
### 構造化データ
- 数値データやカテゴリカルなデータがメインのため、これらのデータは簡単に統計関数に入力できます。
- 数値データはエンコーディングは不要ですし、カテゴリカルなデータも簡単に量的データ(ベクトル)に変換できるので、構造化データについてはほとんど頭を悩ませる必要はありません。

### 非構造化データ
- 画像データはもともとピクセル値の配列で体系だって作られているデータですから、大した前処理もせずにそのまま関数に入力できます。
- 音声ファイルも量的データへの変換方法はだいたい決まっています。「フーリエ変換」し、音声周波数を取り出して、「スペクトル分析」をすると数値化が可能です。「フーリエ変換」や「スペクトル分析」など小難しい名前がでてきますが、今時は便利がAPIが沢山あり、数行のコードで簡単に処理できますので心配無用です。
- 文章データも当然数値化は可能です。一般に想像する1とか2という数値ではなく「ベクトル」(もしくは「埋め込み」)という数値表現です。
- GPT結局は「文章というデータをどのように良質なベクトルに変換するか」というのが本質なように思います。

# 統計関数
- 自然言語のような非構造化データを扱うアルゴリズムとしてはやはりニューラルネットワークということになります。いわゆる深層学習で使われるアルゴリズムです。
![image](https://github.com/user-attachments/assets/cee953f9-193c-4f5b-9621-749bd4677363)
- 目的に応じた様々なタイプのネットワークが開発されています。
  - CNN(Convolution Neural Network)画像分類や物体検出などのタスクはCNN(Convolution Neural Network)を使うことで精度が確保できることが多く、現在はこの一択という感じです。
  - GAN(Generative Adversarial Network)また、画像生成はGAN(Generative Adversarial Network)が元祖ということで、こちらも技術革新著しい生成AIの分野でよく使われるアルゴリズムになります。
  - RNN,そして、主に、自然言語をデータとして扱うアルゴリズムとしてRNNがあります。今となってはGPTの前世代の技術となりますが、この時代の技術を概要だけでも知っておくことはGPTの理解に大きく役立ちます。新しい技術とは過去の技術の課題を改善しながら発展してゆくケースが多く、自然言語処理の世界もまた然りです。

# RNN(Recurrent Neural Network)
RNNは主に、自然言語の深層学習に効果を発揮するアルゴリズムです。入力された文章を単語に区切り、その単語をニューラルネットワークで計算し、その計算結果を次の単語の計算に利用するという処理を、文章の全ての単語に再帰的(Recurrent)に繰り返すニューラルネットワーク、ということでRNN(Recurrent Neural Network)と呼ばれています。
![image](https://github.com/user-attachments/assets/7d58f0e8-1b65-4154-85da-1c00ca23c883)

# RNNの課題
作为能够表达单词之间关系和语境特征的算法，这一领域长期以来一直由它引领，但是RNN有以下两大弊端

- 長期記憶の仕組みがない
  - これが意味するところは、RNNには長文を学習させることに限界があるということです。人間も言語を学習する際、長文を扱うことは重要ですよね。例えば英語学習を例にあげると。「私は学校に行きました。」「私は病院に行きました。」という単純な英文をたくさん学習したところで、長文を理解すること、話せるようになることは難しいと感覚的にわかります。
  - ところが、この状態から関係代名詞という文法を学習したとします。それにより、「私は東京駅の近くにある大きな病院に行きました。」という少し長い文章を理解することができます。コンピュータの言語認識もこれに非常に似ており、長文データの学習が非常に有効だということは、過去の研究で証明されています。
- 並列処理ができない
  - RNNの絵を見ていただくとわかりますが、RNNでは、一つ一つの単語の計算は逐次処理となります。「日本」という単語を処理した後、その処理結果を使って、次の単語「で」の処理を行うということを繰り返します。つまり前の単語の計算が終わらないと、次の単語の計算が始められないのです。したがって、文章内の全ての単語を同時に計算するということができません。
  - この制約により、RNNで大規模なデータを学習させるためには膨大な時間を要することとなり、事実上、合理的な時間で精度の高い推論モデルを作ることが不可能でした。
- 尽管存在这样的缺点，RNN仍然是一种具有捕捉单词之间关系和语境特征的大优点的算法，因此并不容易被其他算法替代。因此，在使用RNN的同时，如何解决这些问题成为了一个方向，出现了**LSTM**和**Attention**等主要技术。

# LSTM(Long Short Term Memory)
RNNのネットワークを改造して、長期記憶の仕組みを組み込んでしまおう、というのがLSTMです。(つまりLSTMもニューラルネットワークの一種です。)
![image](https://github.com/user-attachments/assets/a3cceff5-df4e-4cde-b156-59a41076d1d1)

# Attention
Attention機構は「文章内のどの単語に注意を払うべきか」という点に着目し、各単語に重み付をするような処理を行い、各単語間の関係性や文脈の特徴を表す機構です。各単語間の関係性や文脈の特徴を表す機構という観点ではRNNもそうですよね？と思われますよね。そうです同じなので、AttentionはRNNの補助的な役割として、RNNと併用する形で実装されていました。
![image](https://github.com/user-attachments/assets/bb22c4e3-e964-4784-a98b-c7212e75df5d)

# Transformerの登場
ただ、LSTM、Attentionともに、RNNがベースになっています。RNNを使っている限り、並列処理ができない⇒なので大規模データの学習ができない⇒なのでモデルの精度は結局あまり改善されなかった、というのが歴史的な経緯です。

そこで、思い切ってRNNはあきらめ、もともと補助的に組み込まれていたAttentionを、文脈把握と単語間の関係性把握のためのメインの機構としてフル活用することにより、RNNの大きな課題であった並列処理と長期記憶の問題を解決したTransformerが開発されました。

# GPTの登場
実はこのTransformerこそがGPTです。GPTはGenerative Pretrained Transformerの略。
- Generative　⇒　生成的という意味を持ち、文章生成能力が高いということを指しています。
- Pretrained　⇒　事前学習済み、つまり、Open AI社によって、膨大な学習データで既に学習済のモデルだということを指しています。
- Transformer　⇒　その学習がTransformerで行われたということで

ちなみに、Google社のBERTもTransformerで学習したモデルです。BERTはBidirectional Encoder Representations from Transformersの略となり、こちらも、Bidirectional Encoder Representations(双方向のエンコード表現)がBERTの技術的特徴(文章を文頭と文末から双方向に学習するような仕組み)を表したものだということになります。
つまり、TransformerとGPT、BERTさらに、両社の関連サービスの関係は以下のようになると思います。
![image](https://github.com/user-attachments/assets/6f70cda8-aa2c-4f62-8ce9-17271eaf462c)

# Transformerの仕組み
### エンコーダーとデコーダー
![image](https://github.com/user-attachments/assets/b41427b0-fdfc-4349-81f3-d06470e6acc8)
- そして、Encoderで作られたベクトルはDecoderに入力されます。Decoderはこの入力文章のベクトルを受けて、その文章の後に続く単語(関連性の高い単語)を推論する計算を行い、最終的に最も可能性の高い単語を出力します。
- 自然言語のベクトル化は、画像データや音声データのようなシンプルな手法では実現できません。これらの文章とベクトルの変換処理の過程で、文脈や単語間の関係性をベクトルに反映するための様々な処理があります。

# エンコーダー側の処理
![image](https://github.com/user-attachments/assets/1eae5a47-245d-43ca-90e8-3dd36758ad71)
1. Input Embedding
2. Positional Encoding
3. Encoder Attention (Self Attention)

# デコーダー側の処理
![image](https://github.com/user-attachments/assets/a5b9fec1-16cf-42a7-8663-8b5fd9c0c36b)

4. デコーダーでのSelf Attention
  - エンコーダーとデコーダでSelf Attentionを行う目的が違うということです。
  - エンコーダーは入力文全体の情報を把握できるようにエンコードするのに対して、デコーダーは文脈や依存関係を考慮して次の単語を生成する必要があります。
![image](https://github.com/user-attachments/assets/883e9e4b-22c4-4ddf-abea-3d6069bbf952)

5. デコーダーでのCross Attention
  - エンコーダーが行ったSelf Attentionの結果も考慮したいため、Cross Attentionと呼ばれる別のタイプのAttentionの計算を行います。
![image](https://github.com/user-attachments/assets/d902f73a-2fdc-4d3a-bb76-2ce039617441)


# 長文を出力できる仕組み
![image](https://github.com/user-attachments/assets/1371d23e-50ba-4c59-a147-3cb85c534948)

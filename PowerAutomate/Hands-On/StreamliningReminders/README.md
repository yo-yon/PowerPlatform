# アンケートリマインド業務効率化(ようのアプローチ編)
![panda](https://user-images.githubusercontent.com/62197237/180122643-66576aa3-1782-443f-b58c-4b358e95fb15.png)


皆さんは業務でアンケートを取ったりすることはありますか？  
Microsoft Formsを利用すると簡単にアンケートの作成ができるし、社内の人間に対してのアンケートだった場合、ユーザー名も自動で取得できるので私は非常に重宝しています。  
    
ただ、注意しなきゃいけないのは回答期限が少し先に設定されているアンケートです。  
リマインダーをかけるにしても往々にして忘れがちです。  
以前気ままに勉強会#17でわたるふさん(@wataruf01)が「Power Automateで提出物の催促を自動化した話」をしてくれました。  
  
収集する側は結構未回答者の抽出に苦労するんですよね。  
わたるふさんは「いいね」した人を集計して「いいね」していない人に対してチャネルでメンションするというアプローチをしていました。  
<img src="https://user-images.githubusercontent.com/62197237/180134921-d8034425-5fa2-42d3-9093-d677f242f9c2.png" width="500px" />  
わたるふさんの資料は[こちら！](https://www.docswell.com/assets/libs/docswell-embed/docswell-embed.min.js  )  

  
今回はわたるふさんと基本的なアプローチは同じですが少し違った方法についてハンズオンをしていきます。  
内容としては下記のとおりです。
- 未回答者の洗い出しフロー作成  
- アンケートを出したあと定期的に未回答者にのみリマインドを飛ばすフロー作成    
  
***
今回のアンケートリマインダーフローを使うストーリーですがこんな感じです。  
### ストーリー
月曜日にアンケート配信し回答の締め切りは週末の金曜日とする。  
#### リマインド条件
- 3日後の水曜日と締切日の金曜日の9:00にリマインドを配信  
- アンケート未回答者に対してメンションし、個別チャットでリマインドを飛ばす。  
- 対象者は300名  
*** 
  
### 事前準備編
#### アンケート作成(Microsoft Forms)
- タイトル  
  0220730デモリマインド効率化デモ  
- 質問  
  Power Automateは楽しいですか(選択肢型)  
  <img src="https://user-images.githubusercontent.com/62197237/180107987-3f492d23-654a-4d54-918c-c53a5ecd5b94.png" width="500px" />

#### フロー結果の受け皿用Excelデータ
- アンケート対象者の一覧  
  User列：対象者のメールアドレスを入力  
  DisplayName列：対象者の氏名を入力  
  Status列：フローで自動記入  
  Remaind列：フローで自動記入  
  ※テーブル化し、テーブル名を「remind」にする  
  <img src="https://user-images.githubusercontent.com/62197237/180106601-b38635b9-568f-4142-a728-cd2b07ee873c.png" width="500px" />  
- FormsURLシート  
  TableKey列：「1」を入力  
  Forms_URL列：Forms URLを貼り付け  
  ※テーブル化し、テーブル名を「info」にする  
  <img src="https://user-images.githubusercontent.com/62197237/180107293-0c35ffbe-4ae5-42ec-9276-2564a7afc1c8.png" width="500px" />
***  
  
### フロー作成編
#### アンケート回答結果取得フロー作成
- まずマイフローを選択し「新しいフロー」から「自動化したクラウドフロー」をクリック  
  <img src="https://user-images.githubusercontent.com/62197237/180109627-48abf1c7-ea1a-449e-88b4-75b988bca9c3.png" width="500px" />
- フロー名に任意の名前を付け、Microsoft Formsコネクタの「新しい応答が送信されるとき」を選択し、作成をクリックして下さい。</li>
  <img src="https://user-images.githubusercontent.com/62197237/180109747-e5de80d4-f23c-4424-b6f0-36ce45edd865.png" width="500px" />
- トリガーが作成されるのでフォームIDで先ほど作成したFormsアンケートを選択</li>
- ※選択肢に表示されない場合  
  - FormsのIDをコピー  
    <img src="https://user-images.githubusercontent.com/62197237/180111001-7d49e995-df21-437c-8932-ee8c62ca10ab.png" width="500px" />  
  - カスタム項目の追加」をクリック  
    <img src="https://user-images.githubusercontent.com/62197237/180111005-6a73b5d4-1486-4d4f-9d17-545d5b920de9.png" width="500px" />  
  - フォームIDにコピーした値を貼り付けしOKをクリック  
    <img src="https://user-images.githubusercontent.com/62197237/180110941-1037f78e-2f81-4030-bb6b-f45682074e85.png" width="500px" />  
- 次にMicrosoft Formsコネクタから「応答の詳細を取得する」を選択  
  <img src="https://user-images.githubusercontent.com/62197237/180111324-43de265c-2dcf-45da-84c9-709d210a8513.png" width="500px" />  
- フォームDには先程のFormsアンケートIDを選択もしくは入力、応答IDには「新しい応答が送信されるとき」で作成された応答IDを選択  
  <img src="https://user-images.githubusercontent.com/62197237/180111832-2601ed23-3340-4adf-a356-d4e2a2b53ad4.png" width="500px" />  
- 次にExcel Online(Business)コネクタから「行の更新」を選択  
  <img src="https://user-images.githubusercontent.com/62197237/180113519-3371e881-046c-4229-8c19-922fc3d474fc.png" width="500px" />  
- 下記のように設定  
  場所：受け皿用のExcelデータの格納場所を選択  
  ドキュメンドライブラリ：上記と同様  
  ファイル：対象のファイルを選択  
  テーブル：対象データのテーブルを選択「remind」  
  キー列：Userを選択  
  キー値：Responders' Emailを選択  
  Status：「○」と入力  
  <img src="https://user-images.githubusercontent.com/62197237/180113693-c1eebe1c-a15d-4aaf-a4c7-549231c591d5.png" width="500px" />  
- テスト実行  
  <img src="https://user-images.githubusercontent.com/62197237/180145601-e19ca17f-b0b5-493b-b3d8-e5ea64c2d516.png" width="500px" />  
  <img src="https://user-images.githubusercontent.com/62197237/180145613-2f4c0b35-9393-4526-a78b-bb05ab7d5a02.png" width="500px" />  
- 対象のFormsアンケートに回答し、正常に動作するか確認  
  <img src="https://user-images.githubusercontent.com/62197237/180145867-5f5e2af5-5160-428c-bfb8-13e90f2d475c.png" width="500px" />  
  <img src="https://user-images.githubusercontent.com/62197237/180145880-2889fb95-d83e-477c-9ef1-adb3473acba1.png" width="500px" />  
アンケート回答結果取得フローの作成は以上です。  
***  

#### リマインドフロー作成  
- マイフローを選択し「新しいフロー」から「インスタント クラウドフロー」をクリック  
  <img src="https://user-images.githubusercontent.com/62197237/180115693-3d414023-96cc-414f-9f2d-e554f4e9023c.png" width="500px" />  
- フロー名に任意の名前を付け、「手動でフローをトリガーします」を選択し、作成をクリックして下さい。  
  <img src="https://user-images.githubusercontent.com/62197237/180115699-929aa2bd-6758-4986-8ef0-63a4b19bad87.png" width="500px" />     
- 現在の時刻のアクションを追加します。これは後工程で使用します。  
  特に設定する項目はありません。  
  <img src="https://user-images.githubusercontent.com/62197237/180115719-edd348bd-99da-4f57-ba36-6f3a5538c3ab.png" width="500px" />　　
- 前段で取得した「現在の時刻」はUTC時間なので「タイムゾーンの変換」で日本時間に変換するアクションを追加。  
  下記のように設定  
  基準時間：現在の時刻を選択  
  変換元のタイムゾーン：(UTC)協定世界時  
  変換先のタイムゾーン：(UTC+9:00)大阪、札幌、東京  
  書式設定文字列：お好みで  
  <img src="https://user-images.githubusercontent.com/62197237/180115729-d99ca7f4-cd93-4455-96cf-196c16faabaf.png" width="500px" />  
  <img src="https://user-images.githubusercontent.com/62197237/180115898-137efdfd-6c2b-4ffb-8468-37cd432326c6.png" width="500px" />  
- 次にExcel Online(Business)コネクタから「表内に存在する行を一覧表示」を選択  
  <img src="https://user-images.githubusercontent.com/62197237/180116459-0894b86f-e603-4e44-8f2b-1a60eb93546d.png" width="500px" />
- 下記のように設定  
  場所：受け皿用のExcelデータの格納場所を選択  
  ドキュメンドライブラリ：上記と同様  
  ファイル：対象のファイルを選択   
  テーブル：対象データのテーブルを選択「remind」  
  <img src="https://user-images.githubusercontent.com/62197237/180117629-d465c7ed-a8aa-4b8e-8294-90b216a4d075.png" width="500px" />  
- 「表内に存在する行を一覧表示」の右上の三点リーダーをクリックし「設定」をクリック  
  <img src="https://user-images.githubusercontent.com/62197237/180117657-998b02bb-df1a-46b7-873b-f5bf57713513.png" width="500px" />  
- 改ページを「オン」にして、しきい値を「500」にし、完了をクリック  
  <img src="https://user-images.githubusercontent.com/62197237/180117678-7f686bd3-61d2-457e-a95a-fd0c359b8a27.png" width="500px" />  
- 「詳細オプションを表示する」をクリックしてにして、フィルタークエリに「Status ne '○'」と入力  
  <img src="https://user-images.githubusercontent.com/62197237/180117686-42aa4625-8462-47a9-8cbd-192b3023369b.png" width="500px" />  
- 次にExcel Online(Business)コネクタから「行の取得」を選択  
  <img src="https://user-images.githubusercontent.com/62197237/180118917-ee29b9cd-8d92-4517-ab8d-d8bb9d6270f0.png" width="500px" />  
- 下記のように設定  
  場所：受け皿用のExcelデータの格納場所を選択  
  ドキュメンドライブラリ：上記と同様  
  ファイル：対象のファイルを選択  
  テーブル：対象データのテーブルを選択「info」  
  キー列：TableKeyを選択  
  キー値：「1」と入力  
  <img src="https://user-images.githubusercontent.com/62197237/180118923-9fd2278d-9d86-4169-9e11-524b2310d146.png" width="500px" />  
- コントロールから「Apply to each」を選択  
  <img src="https://user-images.githubusercontent.com/62197237/180119926-6892996c-ed28-4274-ae80-cf547b2eadbd.png" width="500px" />  
- 「以前の手順から出力を選択」で「表内に存在する行を一覧表示」から「value」を選択  
  <img src="https://user-images.githubusercontent.com/62197237/180119973-d4e21347-324f-4364-a82f-e4982affba16.png" width="500px" />  
  <img src="https://user-images.githubusercontent.com/62197237/180119984-26ac333e-bfcd-484f-bf99-822e98cc67e5.png" width="500px" />  
- 「Apply to each」の中の「アクションの追加」をクリック  
  <img src="https://user-images.githubusercontent.com/62197237/180120372-5145acc9-2191-474d-b6f7-18f4e002d0a8.png" width="500px" />  
- Microsoft Teamsコネクタから「ユーザーの@mentionトークンを取得する」を選択  
  <img src="https://user-images.githubusercontent.com/62197237/180121667-1de86767-533c-4bb0-bec2-1d0ca87be516.png" width="500px" />  
- 「ユーザー」欄に「表内に存在する行を一覧表示」から「user」を選択  
  <img src="https://user-images.githubusercontent.com/62197237/180121677-c458beb3-8e5a-4246-8aac-da625674a8a4.png" width="500px" />  
- 「Apply to each」の中の「アクションの追加」をクリック  
- Microsoft Teamsコネクタから「チャットやチャネルにアダプティブカードを投稿する」を選択  
  <img src="https://user-images.githubusercontent.com/62197237/180121688-19a8e895-a9da-4316-a982-51d86ec89627.png" width="500px" />  
- 下記のように設定  
  投稿者：「フローボット」を選択  
  投稿先：「フローボットとチャットするを選択」  
  Recipient：「表内に存在する行を一覧表示」から「user」を選択  
  アダプティブカード：任意で設定  
  <img src="https://user-images.githubusercontent.com/62197237/180141357-4ae94acf-82e6-4f99-9b16-4a6b7d749f70.png" width="500px" /></p>
  [※参考：アダプティブカードJSON](https://gist.github.com/yo-yon/f5453a79a3812c3e32c98c56b2915eec)  
- 一旦、フロー結果の受け皿用ExcelデータのStatus列で自分の行を残して「○」を入力し、テスト実行  
  <img src="https://user-images.githubusercontent.com/62197237/180143563-61614971-4a84-46e0-9eee-8a8a77e5f7e7.png" width="500px" /></p>
 
以上でハンズオン終了です。  
お疲れ様でした！  
<img src="https://user-images.githubusercontent.com/62197237/180142085-0c65b3a5-caf9-4001-b1d9-367ed83c5ba4.png" width="500px" />
<img src="https://user-images.githubusercontent.com/62197237/180146269-738c5dc7-bd2d-406f-b573-e5751db7d064.png" width="500px" />  

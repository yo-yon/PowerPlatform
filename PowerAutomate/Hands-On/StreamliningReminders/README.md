<h1>アンケートリマインド業務効率化(ようのアプローチ編)</h1>
<p>皆さんは業務でアンケートを取ったりすることはありますか？</p>
<p>Microsoft Formsを利用すると簡単にアンケートの作成ができるし、社内の人間に対してのアンケートだった場合、ユーザー名も自動で取得できるので私は非常に重宝しています。<br />
  ただ、注意しなきゃいけないのは回答期限が少し先に設定されているアンケートです。<br />
  リマインダーをかけるにしても往々にして忘れがちです。<br />そんなわけでアンケートを出したあと定期的に未回答者にのみリマインドを飛ばすフローを作ってみましょう。 <br /><br />
</p>
<p>今回のアンケートリマインダーフローを使うストーリーですがこんな感じです。</p>
<h3>ストーリー</h3>
<p>月曜日にアンケート配信し回答の締め切りは週末の金曜日とする。</p>
<h4>リマインド条件</h4>
<ul>
<li>3日後の水曜日と締切日の金曜日の9:00にリマインドを配信</li>
<li>アンケート未回答者に対してメンションし、個別チャットでリマインドを飛ばす。</li>
<li>対象者は300名</li>
</ul>
<h3>事前準備編</h3>
<h4>アンケート作成(Microsoft Forms)</h4>
<ul>
<li>タイトル<br />  20220730デモリマインド効率化デモ<br /></li>
<li>質問<br />  Power Automateは楽しいですか(選択肢型)<br /></li>
<p><img src="https://user-images.githubusercontent.com/62197237/180107987-3f492d23-654a-4d54-918c-c53a5ecd5b94.png" width="500px" /></p>
</ul>
<h4>フロー結果の受け皿用Excelデータ</h4>
<ul>
<li>アンケート対象者の一覧<br />User列：対象者のメールアドレスを入力<br />  DisplayName列：対象者の氏名を入力<br />  Status列：フローで自動記入<br />  Remaind列：フローで自動記入<br />  ※テーブル化し、テーブル名を「remind」にする</li>
<p><img src="https://user-images.githubusercontent.com/62197237/180106601-b38635b9-568f-4142-a728-cd2b07ee873c.png" width="500px" /></p>
<li>FormsURLシート<br />TableKey列：「1」を入力<br />  Forms_URL列：Forms URLを貼り付け<br />  ※テーブル化し、テーブル名を「info」にする</li>
<p><img src="https://user-images.githubusercontent.com/62197237/180107293-0c35ffbe-4ae5-42ec-9276-2564a7afc1c8.png" width="500px" /></p>
</ul>
<h3>フロー作成編</h3>
<h4>アンケート回答結果取得フロー作成</h4>
<ul>
<li>まずマイフローを選択し「新しいフロー」から「自動化したクラウドフロー」をクリック</li>
<p><img src="https://user-images.githubusercontent.com/62197237/180109627-48abf1c7-ea1a-449e-88b4-75b988bca9c3.png" width="500px" /></p>
<li>フロー名に任意の名前を付け、Microsoft Formsコネクタの「新しい応答が送信されるとき」を選択し、作成をクリックして下さい。</li>
<p><img src="https://user-images.githubusercontent.com/62197237/180109747-e5de80d4-f23c-4424-b6f0-36ce45edd865.png" width="500px" /></p>
<li>トリガーが作成されるのでフォームIDで先ほど作成したFormsアンケートを選択</li>
<li>※選択肢に表示されない場合<br />
  <p>FormsのIDをコピー<br />
  <img src="https://user-images.githubusercontent.com/62197237/180111001-7d49e995-df21-437c-8932-ee8c62ca10ab.png" width="500px" /></p>
  <p>「カスタム項目の追加」をクリック<br />
  <img src="https://user-images.githubusercontent.com/62197237/180111005-6a73b5d4-1486-4d4f-9d17-545d5b920de9.png" width="500px" /></p>
  <p>フォームIDにコピーした値を貼り付けしOKをクリック<br />
  <img src="https://user-images.githubusercontent.com/62197237/180110941-1037f78e-2f81-4030-bb6b-f45682074e85.png" width="500px" /></p>
<li>次にMicrosoft Formsコネクタから「応答の詳細を取得する」を選択</li>
<p><img src="https://user-images.githubusercontent.com/62197237/180111324-43de265c-2dcf-45da-84c9-709d210a8513.png" width="500px" /></p>
<li>フォームDには先程のFormsアンケートIDを選択もしくは入力、応答IDには「新しい応答が送信されるとき」で作成された応答IDを選択</li>
<p><img src="https://user-images.githubusercontent.com/62197237/180111832-2601ed23-3340-4adf-a356-d4e2a2b53ad4.png" width="500px" /></p>  
<li>次にExcel Online(Business)コネクタから「行の更新」を選択</li>
<p><img src="https://user-images.githubusercontent.com/62197237/180111832-2601ed23-3340-4adf-a356-d4e2a2b53ad4.png" width="500px" /></p>  
<li>下記のように設定<br />場所：受け皿用のExcelデータの格納場所を選択<br />ドキュメンドライブラリ：上記と同様<br />ファイル：対象のファイルを選択<br />テーブル：対象データのテーブルを選択<br />キー列：Userを選択<br />キー値：Responders' Emailを選択<br />Status：「○」と入力<br /></li>
<p><img src="https://user-images.githubusercontent.com/62197237/180111832-2601ed23-3340-4adf-a356-d4e2a2b53ad4.png" width="500px" /></p>  


</ul>
<h4>リマインドフロー作成</h4>


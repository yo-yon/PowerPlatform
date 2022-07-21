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
<li>アンケート対象者の一覧<br />User列：対象者のメールアドレスを入力<br />  DisplayName列：対象者の氏名を入力<br />  Status列：フローで自動記入<br />  Remaind列：フローで自動記入</li>
<p><img src="https://user-images.githubusercontent.com/62197237/180106601-b38635b9-568f-4142-a728-cd2b07ee873c.png" width="500px" /></p>
<li>FormsURLシート<br />TableKey列：「1」を入力<br />  Forms_URL列：Forms URLを貼り付け</li>
<p><img src="https://user-images.githubusercontent.com/62197237/180107293-0c35ffbe-4ae5-42ec-9276-2564a7afc1c8.png" width="500px" /></p>
</ul>
<h3>フロー作成編</h3>
<h4>アンケート回答結果取得フロー作成</h4>
<ul>
<li>まずマイフローを選択し「新しいフロー」から「自動化したクラウドフロー」をクリック</li>
<p><img src="https://user-images.githubusercontent.com/62197237/180109627-48abf1c7-ea1a-449e-88b4-75b988bca9c3.png" width="500px" /></p>
<li>フロー名に任意の名前を付け、Microsoft Formsコネクタの「新しい応答が送信されるとき」を選択し、作成をクリックして下さい。</li>
<p><img src="https://user-images.githubusercontent.com/62197237/180109594-309c0f21-1f19-4898-ab12-e8c3f1f82ab9.png" width="500px" /></p>
<li>トリガーが作成されるのでフォームIDで先ほど作成したFormsアンケートを選択して下さい。</li>
<li>次にMicrosoft Formsコネクタから「応答の詳細を取得する」を選択し、フォームDには先程のFormsアンケート、応答IDには「新しい応答が送信されるとき」で作成された応答IDを選択して下さい。</li>
<li>次にExcel Online(Business)コネクタから「行の更新」を選択し、下記のように設定します。<br />場所：受け皿用のExcelデータの格納場所を選択<br />ドキュメンドライブラリ：上記と同様<br />ファイル：対象のファイルを選択<br />テーブル：対象データのテーブルを選択<br />キー列：Userを選択<br />キー値：Responders' Emailを選択<br />Status：回答済みと入力<br /></li>



</ul>
<h4>リマインドフロー作成</h4>


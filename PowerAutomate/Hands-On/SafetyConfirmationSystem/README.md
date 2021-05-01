<p>皆さんこんにちは。<br />業務ハックLabのよ～よんです。</p>
<p>今日はPower Automateを利用して安否確認システムを作ってみたので<br />それについてご案内していきたいと思います。</p>
<p>[:contents]</p>
<h3>概要</h3>
<p>今回使用するのは</p>
<p>・Power Automate<br />・Microsoft Teams<br />・Microsoft Forms</p>
<p>の3つです。</p>
<p>ちなみにPower AutomateはPremiumコネクタであるHTTPコネクタを使用するので有料版のライセンス(Micosoft365付属のライセンスは不可)が必要になりますのでご注意ください。</p>
<p>全体の流れとしてはこんな感じ。</p>
<p><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/y/yo-yon/20210409/20210409083724.png" alt="f:id:yo-yon:20210409083724p:plain" title="" class="hatena-fotolife" itemprop="image" /><br />今回はPower Automateのフローを2パターン作りました。<br />一つは気象庁のホームページからxml情報を取得する方法、もう一つはP2P地震情報さんが提供されているJSON API v2を利用する方法です。<br />気象庁の情報取得ページはこちら</p>
<p><iframe src="https://hatenablog-parts.com/embed?url=http%3A%2F%2Fxml.kishou.go.jp%2Fxmlpull.html" title="気象庁 | 気象庁防災情報XMLフォーマット形式電文（PULL型）" class="embed-card embed-webcard" scrolling="no" frameborder="0" style="display: block; width: 100%; height: 155px; max-width: 500px; margin: 10px 0px;"></iframe><cite class="hatena-citation"><a href="http://xml.kishou.go.jp/xmlpull.html">xml.kishou.go.jp</a></cite></p>
<p><br />P2P地震情報さんのJSON API v2ページはこちら</p>
<p><iframe src="https://hatenablog-parts.com/embed?url=https%3A%2F%2Fwww.p2pquake.net%2Fjson_api_v2%2F" title="JSON API v2 · P2P地震情報" class="embed-card embed-webcard" scrolling="no" frameborder="0" style="display: block; width: 100%; height: 155px; max-width: 500px; margin: 10px 0px;"></iframe><cite class="hatena-citation"><a href="https://www.p2pquake.net/json_api_v2/">www.p2pquake.net</a></cite></p>
<p>どちらも同じ結果を返すことが出来ますが、JSON API v2を利用させて頂く形にするとPower Automateで組むアクション数が少なくでき、すっきりさせる事もできます。</p>
<h3>注意事項</h3>
<p>下記についてご注意願います。</p>
<ul>
<li>今回作成したこの安否確認システムでは気象庁ホームページから情報を入手しています。使用に際しては<strong><span style="color: #ff0000;">気象庁の情報を利用している旨の表示</span></strong>が必要となります。<a href="https://www.jma.go.jp/jma/kishou/info/coment.html">気象庁 | 著作権・リンク・個人情報保護について</a></li>
<li>あくまで個人的に作成したものですので無保証です。<span style="color: #ff0000;"><strong>利用によるいかなる損害についても、一切の責任を負いません。</strong></span><br />また、<span style="color: #ff0000;"><strong>情報の正確性も一切保証致しません</strong></span>。</li>
</ul>
<p>言いましたよ。言いましたからね！</p>
<h3>Power Automateの構成</h3>
<p>今回はフローの中のアクションに設定している関数など細かい内容は書きません。<br />作る際に参考にしたページがいくつか有るのですがそちらのリンクを張っておきますので是非チャレンジしてみてください。<br />では早速フローの構成に行ってみましょう！！</p>
<h4>気象庁ホームページxml情報取得パターン</h4>
<p>色々とフィルタを掛けたりしてるので結構長いフローになってます。<br />全体を見るとこんな感じ。</p>
<p><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/y/yo-yon/20210409/20210409120851.png" alt="f:id:yo-yon:20210409120851p:plain" title="" class="hatena-fotolife" itemprop="image" /></p>
<p><br />ちょっと分解しつつ、動きの部分を説明していきます</p>
<h5>トリガー～タイムゾーンの変換</h5>
<p>トリガーはスケジュールの「繰り返し」を使用しています。<br />あまり高頻度で繰り返しを設定すると気象庁のサーバーへの負荷が高くなるとともにAPIコール数も消費してしまいますのでご注意願います。<br />特に気象庁サーバーへの負荷に関してはしっかり考慮するようにしてください。</p>
<p><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/y/yo-yon/20210409/20210409095514.png" alt="f:id:yo-yon:20210409095514p:plain" title="" class="hatena-fotolife" itemprop="image" /></p>
<h5>変数の定義</h5>
<p>後工程で使う変数を定義します。</p>
<p><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/y/yo-yon/20210409/20210409100149.png" alt="f:id:yo-yon:20210409100149p:plain" title="" class="hatena-fotolife" itemprop="image" /></p>
<h5>フィードの取得～作成-一番最初のアレイのみ</h5>
<p>RSSコネクタで気象庁のホームページからフィード項目を取得しています。<br />ちなみに今回はAtomフィードの高頻度、「地震火山」の情報を使用しています。</p>
<p><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/y/yo-yon/20210409/20210409102354.png" alt="f:id:yo-yon:20210409102354p:plain" title="" class="hatena-fotolife" itemprop="image" /></p>
<h5>条件分岐</h5>
<p>フィードからデータを取得した際に様々な条件でフィルタを掛けますが条件に合致せず、データが空白だった場合に分岐するよう設定をします。</p>
<p><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/y/yo-yon/20210409/20210409102815.png" alt="f:id:yo-yon:20210409102815p:plain" title="" class="hatena-fotolife" itemprop="image" /></p>
<h5>はいの場合</h5>
<p>上記の条件に合致する(データが空白)場合は最新情報がない状態なので処理時間のみをExcelデータに更新してフローを終了します。</p>
<p><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/y/yo-yon/20210409/20210409103631.png" alt="f:id:yo-yon:20210409103631p:plain" title="" class="hatena-fotolife" itemprop="image" /></p>
<h5>いいえの場合：JSONの解析-最新情報を分解～条件分岐</h5>
<p>上記条件に合致しない(データが空白ではない)場合は最新情報がある状態ですので次の工程に進みます。<br />具体的には取得した情報の中に地震情報の詳細が記載されたxmlデータへのURLが存在するので、そのデータをHTTPコネクタでGETしています。<br />また最後に条件アクションですでに取得済みの管理ID(取得したデータが同一の地震情報なのかの判定)、安否確認を投稿する最大震度の下限値を設定します。</p>
<p><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/y/yo-yon/20210409/20210409104107.png" alt="f:id:yo-yon:20210409104107p:plain" title="" class="hatena-fotolife" itemprop="image" /></p>
<p>条件アクションの中はこのような感じになっています。</p>
<p><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/y/yo-yon/20210409/20210409100708.png" alt="f:id:yo-yon:20210409100708p:plain" title="" class="hatena-fotolife" itemprop="image" /></p>
<h5>はいの場合</h5>
<p>上記の条件に合致する(すでに取得、投稿済みのデータもしくは最大震度しきい値未満の地震)場合は最新情報がない状態なので処理時間のみをExcelデータに更新してフローを終了します。</p>
<p><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/y/yo-yon/20210409/20210409104140.png" alt="f:id:yo-yon:20210409104140p:plain" title="" class="hatena-fotolife" itemprop="image" /></p>
<h5>いいえの場合：行の更新～アダプティブカード投稿</h5>
<p>上記条件に合致しない(まだ投稿していない情報かつ最大震度しきい値以上)場合は最新情報がある状態ですので次の工程に進みます。<br />Excelデータへ処理日時と管理IDを更新しAdaptive cardsを特定のチャネルへ投稿します。</p>
<p><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/y/yo-yon/20210409/20210409104734.png" alt="f:id:yo-yon:20210409104734p:plain" title="" class="hatena-fotolife" itemprop="image" /></p>
<p>気象庁ホームページxml情報取得するパターンのフローは以上です。</p>
<h4>P2P地震情報 JSON API v2利用パターン</h4>
<p>こちらの全体フローはこんな感じ。</p>
<p><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/y/yo-yon/20210409/20210409121109.png" alt="f:id:yo-yon:20210409121109p:plain" title="" class="hatena-fotolife" itemprop="image" /></p>
<p>データの取得方法以外はほぼ同じなので割愛。</p>
<p>地震情報のデータ取得はここだけでやっています。</p>
<p><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/y/yo-yon/20210409/20210409121611.png" alt="f:id:yo-yon:20210409121611p:plain" title="" class="hatena-fotolife" itemprop="image" /></p>
<p>前段では地震詳細情報を取得するのに「フィードの取得～作成-一番最初のアレイのみ」と「JSONの解析-最新情報を分解～条件分岐」の「JSONの解析」アクション、「HTTP」アクションまで必要でした。<br />この部分ですね。</p>
<p><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/y/yo-yon/20210410/20210410110214.png" alt="f:id:yo-yon:20210410110214p:plain" title="" class="hatena-fotolife" itemprop="image" /></p>
<p><br /><br /></p>
<p>これがAPIを使うとたったこれだけ。</p>
<p><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/y/yo-yon/20210409/20210409113655.png" alt="f:id:yo-yon:20210409113655p:plain" title="" class="hatena-fotolife" itemprop="image" /></p>
<p>偉大ですね。JSON API！！<br />公開していただいているP2P地震情報さんに感謝です。</p>
<h4>2021/4/10追記</h4>
<p>Microsoft MVPのHiroさんからHTTPコネクタを使用しない方法があることを教えて頂きました！！<br />早速試してみたのですが問題なく動いたので追記しておきます！<br />方法ですが<br />HTTPコネクタを</p>
<p><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/y/yo-yon/20210410/20210410110237.png" alt="f:id:yo-yon:20210410110237p:plain" title="" class="hatena-fotolife" itemprop="image" /></p>
<p>この形にするだけ！！</p>
<p><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/y/yo-yon/20210410/20210410110256.png" alt="f:id:yo-yon:20210410110256p:plain" title="" class="hatena-fotolife" itemprop="image" /></p>
<p>今回HTTPコネクタはGETのために使用していたのでHiroさんに教えていただいた方法はドンピシャでハマりました！<br />こんな方法があったなんてびっくりです。<br />さすがHiroさん！</p>
<p><iframe src="https://hatenablog-parts.com/embed?url=https%3A%2F%2Fqiita.com%2Fh-nagao%2Fitems%2F65f498d9d4e4f1a5fa50%3Ffbclid%3DIwAR0ydt4c9dNx9TuddDlUj3--IRgrq2JQKY49Ed67zgfb9fzbx8G9X3F2D-A" title="[TIPS] Power AutomateでHTTPを使わずにHTMLを取得する - Qiita" class="embed-card embed-webcard" scrolling="no" frameborder="0" style="display: block; width: 100%; height: 155px; max-width: 500px; margin: 10px 0px;"></iframe><cite class="hatena-citation"><a href="https://qiita.com/h-nagao/items/65f498d9d4e4f1a5fa50?fbclid=IwAR0ydt4c9dNx9TuddDlUj3--IRgrq2JQKY49Ed67zgfb9fzbx8G9X3F2D-A">qiita.com</a><br /></cite></p>
<p>HiroさんのブログはPower Platformで何かを構築したりする上でめちゃくちゃ有用な情報が載ってるのでぜひ一度ご覧ください！！</p>
<p><iframe src="https://hatenablog-parts.com/embed?url=https%3A%2F%2Fmofumofupower.hatenablog.com%2F" title="MoreBeerMorePower" class="embed-card embed-webcard" scrolling="no" frameborder="0" style="display: block; width: 100%; height: 155px; max-width: 500px; margin: 10px 0px;"></iframe><cite class="hatena-citation"><a href="https://mofumofupower.hatenablog.com/">mofumofupower.hatenablog.com</a></cite></p>
<h4>Power BIのストリーミングデータセットへデータを格納する方法</h4>
<p>では次に安否報告結果をリアルタイムで確認するためのPower BIの作成です。<br />まず最初にFormsで報告した内容を格納するための、ストリーミングデータセットを作成します。</p>
<h5>ストリーミングデータセットの作成</h5>
<ol>
<li>Power BIでデータセットを作成したいワークスペースを選択し「新規」をクリックします。<br /><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/y/yo-yon/20210409/20210409132119.png" alt="f:id:yo-yon:20210409132119p:plain" title="" class="hatena-fotolife" itemprop="image" /></li>
<li>「ストリーミングデータセット」をクリックします。<br />
<p><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/y/yo-yon/20210409/20210409132313.png" alt="f:id:yo-yon:20210409132313p:plain" title="" class="hatena-fotolife" itemprop="image" /></p>
</li>
<li>「API」を選択し、「次へ」をクリックします。<br />
<p><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/y/yo-yon/20210409/20210409132510.png" alt="f:id:yo-yon:20210409132510p:plain" title="" class="hatena-fotolife" itemprop="image" /></p>
</li>
<li>安否報告フォームの設問に併せて値を入れるための枠の部分を作成します。<br /> (下記例参照)<br />全て入力が終わったら必ず「履歴データの解析」をオンにして「作成」をクリックします。(オンにしないとリアルタイム集計できません。)<br />
<p><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/y/yo-yon/20210409/20210409133502.png" alt="f:id:yo-yon:20210409133502p:plain" title="" class="hatena-fotolife" itemprop="image" /></p>
これで報告データを格納するための場所は完成です。</li>
</ol>
<h5>ストリーミングデータセットにデータを格納するフロー</h5>
<p>次にPower Automate でPower BIのストリーミングデータセットにForms回答結果のデータを格納するフローを構築します。<br />「Formsの新しい応答が送信されるとき」というものが有るのでこれをトリガーとします。<br />後は応答の詳細を取得し、Formsに回答をしたユーザー情報などを検索した上で、「データセットに行を追加します」というコネクタの設定を行います。</p>
<p><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/y/yo-yon/20210409/20210409112622.png" alt="f:id:yo-yon:20210409112622p:plain" title="" class="hatena-fotolife" itemprop="image" /></p>
<p>さあ、これで設定は完了しました。<br />実際に動いているところですがこんな感じになります。</p>
<blockquote class="twitter-tweet">
<p dir="ltr" lang="ja">Power Automate×Teams×Forms×Power BIで<br />安否確認システムを作ってみました！！<br />報告者数が1から増えないのは報告者を一意のカウントとしてるからです。<br />色々できて面白いな～<a href="https://twitter.com/hashtag/PowerPlatform?src=hash&amp;ref_src=twsrc%5Etfw">#PowerPlatform</a> <a href="https://twitter.com/hashtag/Micosoft365?src=hash&amp;ref_src=twsrc%5Etfw">#Micosoft365</a> <a href="https://t.co/izip2PJ3WU">pic.twitter.com/izip2PJ3WU</a></p>
— よ～よん@非IT企業の情シス (@Yo_Yon21) <a href="https://twitter.com/Yo_Yon21/status/1380141635107889152?ref_src=twsrc%5Etfw">April 8, 2021</a></blockquote>
<p>Teamsにはこんな感じでAdaptive cardsが送られてきます。</p>
<p><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/y/yo-yon/20210409/20210409194900.png" alt="f:id:yo-yon:20210409194900p:plain" title="" class="hatena-fotolife" itemprop="image" /></p>
<p>確認フォームの集計結果がこんな感じ。</p>
<p><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/y/yo-yon/20210409/20210409195030.png" alt="f:id:yo-yon:20210409195030p:plain" title="" class="hatena-fotolife" itemprop="image" /></p>
<p>これがリアルタイムに更新されていきます。</p>
<h5>ストリーミングデータセットをリセットする方法</h5>
<p>ちなみにストリーミングデータセットに格納されたデータを空にするには下記の手順を行えばリセットができます。</p>
<ol>
<li>データセットを作成したワークスペースで該当するデータセットの作成したストリーミングデータセットの三点リーダーをクリックします。<br />
<p><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/y/yo-yon/20210409/20210409141824.png" alt="f:id:yo-yon:20210409141824p:plain" title="" class="hatena-fotolife" itemprop="image" /></p>
<p><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/y/yo-yon/20210409/20210409141841.png" alt="f:id:yo-yon:20210409141841p:plain" title="" class="hatena-fotolife" itemprop="image" /></p>
</li>
<li>「編集」をクリックします。<br />
<p><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/y/yo-yon/20210409/20210409141857.png" alt="f:id:yo-yon:20210409141857p:plain" title="" class="hatena-fotolife" itemprop="image" /></p>
</li>
<li>「履歴データの解析」をオフにします。(この時点でデータがリセットされます。)
<p><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/y/yo-yon/20210409/20210409141915.png" alt="f:id:yo-yon:20210409141915p:plain" title="" class="hatena-fotolife" itemprop="image" /></p>
</li>
<li>
<p> 再度「履歴データの解析」をオンにして「完了」をクリックします。</p>
(オンにしておかないと次の集計時に反映されないので注意！！)<br />
<p><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/y/yo-yon/20210409/20210409142050.png" alt="f:id:yo-yon:20210409142050p:plain" title="" class="hatena-fotolife" itemprop="image" /></p>
</li>
</ol>
<p>当然のことですが集計を行っている最中にここの設定をオフにするとデータが全てなくなるのでご注意を！</p>
<p><br />ざっと書きましたが今回作った安否確認システムの仕組みはこのような形で作成しました。<br />冒頭の注意事項でも上げたとおりあくまで個人的に作成したものですので無保証です。利用によるいかなる損害についても、一切の責任を負いません。<br />また、情報の正確性も一切保証致しません。<br />作成する際は自己責任でお願い致します。</p>
<p>
<script async="" src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
</p>
<h4>参考とさせていただいたホームページ、ブログなど</h4>
<h5>Microsoft Docs</h5>
<p><iframe src="https://hatenablog-parts.com/embed?url=https%3A%2F%2Fdocs.microsoft.com%2Fja-jp%2Fpower-automate%2Fdesktop-flows%2Factions-reference%2Fxml" title="XML | Microsoft Docs - Power Automate" class="embed-card embed-webcard" scrolling="no" frameborder="0" style="display: block; width: 100%; height: 155px; max-width: 500px; margin: 10px 0px;"></iframe><cite class="hatena-citation"><a href="https://docs.microsoft.com/ja-jp/power-automate/desktop-flows/actions-reference/xml">docs.microsoft.com</a></cite></p>
<h5>P2P地震情報さんのホームページ・ブログ</h5>
<p><iframe src="https://hatenablog-parts.com/embed?url=https%3A%2F%2Fwww.p2pquake.net%2F" title="P2P地震情報" class="embed-card embed-webcard" scrolling="no" frameborder="0" style="display: block; width: 100%; height: 155px; max-width: 500px; margin: 10px 0px;"></iframe></p>
<p><cite class="hatena-citation"><a href="https://www.p2pquake.net/">www.p2pquake.net<br /></a></cite></p>
<p><iframe src="https://hatenablog-parts.com/embed?url=https%3A%2F%2Fp2pquake.hatenablog.jp%2F" title="P2P地震情報 開発ログ" class="embed-card embed-webcard" scrolling="no" frameborder="0" style="display: block; width: 100%; height: 155px; max-width: 500px; margin: 10px 0px;"></iframe><cite class="hatena-citation"><a href="https://p2pquake.hatenablog.jp/">p2pquake.hatenablog.jp</a></cite></p>
<h5>Power BI王子ことMicrosoft MVPの清水 優吾さんのQiita記事</h5>
<p><iframe src="https://hatenablog-parts.com/embed?url=https%3A%2F%2Fqiita.com%2Fyugoes1021%2Fitems%2F10b789e6c29cb367ec99" title="[Power BI Tips] RSS フィードで取れるデータを Power BI レポートにしたいと思った時の Power Automate の使い方 - Qiita" class="embed-card embed-webcard" scrolling="no" frameborder="0" style="display: block; width: 100%; height: 155px; max-width: 500px; margin: 10px 0px;"></iframe><cite class="hatena-citation"><a href="https://qiita.com/yugoes1021/items/10b789e6c29cb367ec99">qiita.com</a></cite></p>
<p><iframe src="https://hatenablog-parts.com/embed?url=https%3A%2F%2Fqiita.com%2Fyugoes1021%2Fitems%2Fea4c96cec9f1c7e47347" title="[Power BI Tips] Power BI でリアルタイムアンケート ～ アンケートを即可視化 2020年版 ～ - Qiita" class="embed-card embed-webcard" scrolling="no" frameborder="0" style="display: block; width: 100%; height: 155px; max-width: 500px; margin: 10px 0px;"></iframe><cite class="hatena-citation"><a href="https://qiita.com/yugoes1021/items/ea4c96cec9f1c7e47347">qiita.com</a></cite></p>
<p> </p>
<p>今回も結構長い記事になってしまいましたね。。。<br />ともあれPower Platformを組み合わせで使用すると色々なことができます。<br />今回はプレミアムコネクタを利用しているので完全にMicosoft365アカウント付随のライセンスのみというわけにはいきませんでしたが、簡易的とはいえ、安否確認システムまで作れちゃうなんてPower Platformのポテンシャルは計り知れないですね！</p>
<p>それでは本日はこのへんで！<br />皆さん良い業務ハックライフを～</p>
<p>

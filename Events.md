## Problem Determination

ここからは、データベースの再起動により、簡単な障害を発生させ、Instanaで アプリケーションの障害がどのようにみえるのか確認していきます。  
処理が流れているなかで、データベースを再起動すれば仕掛かり中の処理はエラーになり、後続の要求は接続待ち、その後タイムアウトのエラーが発生します。それがどのように把握できる確認していきます。
講師の方は、バックエンド のデータベースの再起動を実施してください。

### アプリケーションの問題切り分け

1. ダイレクトの **サマリー＊＊のダッシュボードを開きます。
中央のエラーコール率のグラフに山ができており、エラーが発生していることが分かります。直近のテスト状況が分かりやすいように、右上のボタンで 表示時間を10分や30分などに設定してみてください。 <img width="1914" alt="image" src="https://github.com/iwashinat/InstanaLab/assets/22209835/5539df06-6f43-4d2a-b69b-ff6393467730">
1. エラーコール率のグラフにフォーカスを当て、虫眼鏡アイコンをクリックして、解析画面を表示をします。<img width="1909" alt="image" src="https://github.com/iwashinat/InstanaLab/assets/22209835/0763bfae-23e2-4a96-9bc8-41dd7044b50e">
1. エラーとなった処理の一覧が表示されます。以下の例では仕掛かり中の エラーが１件、接続エラーが132件発生していますね。仕掛かり中でエラーとなったクエリを開きます。<img width="961" alt="image" src="https://github.com/iwashinat/InstanaLab/assets/22209835/1235a322-eb7d-4010-8561-28c3ae514c64">
1. 仕掛かり中でエラーとなったクエリを含む要求が、フロントのWEB、APからバックエンドAPへとどのようなシーケンスで呼び出され、どの処理がエラーになったのかが即座に分かります。　　右側には JDBCドライバーが返しているエラー・メッセージが表示されていますし、実際に投げていたクエリが分かります。アプリケーション側でエラーを吐いていれば、そのログ・メッセージも出力されます（このアプリケーションはログを入れていません）。<img width="969" alt="image" src="https://github.com/iwashinat/InstanaLab/assets/22209835/65867734-98f4-4030-af5b-e6ae1d519590">
1. さきほどの ダッシュボードに戻ります。**エラー・メッセージ＊＊のタブには、要求が返したエラー・メッセージが記録されています。ここのエラー・メッセージのリンクから、このエラー・メッセージを返している要求を確認していくこともできます。<img width="1903" alt="image" src="https://github.com/iwashinat/InstanaLab/assets/22209835/c23e7b94-2ced-406a-afa6-a5c227ce97af">
1. 一つ選んで開いてみましょう<img width="1912" alt="image" src="https://github.com/iwashinat/InstanaLab/assets/22209835/047109b4-99e4-4a13-96bc-794b22189fd5">
1. このエラーを返している要求のトレースが確認できます。Instanaはサンプリングではなく、*全要求トレース＊　ですので、問題が発生した場合であっても、かならずその要求がトレースに含まれており、調査することができます。<img width="1910" alt="image" src="https://github.com/iwashinat/InstanaLab/assets/22209835/e165bf57-c91f-40ee-a08e-1cfbb833555c">

### 問題とインシデント

1. ダイレクトのサマリー画面をみると 左上に黄色い警告が出ていることに気づきます。
Instanaは、サービスの正常性を管理するため、各センサーのゴールデン・シグナル（トランザクション数、応答性能、エラー率など）に、機械学習を適用しています。  
応答性能の急激な低下や悪化、エラー率の急増など、ユーザーに影響を与えている通常の振る舞いと異なる状況を検知し、自動的にイベントを生成します。これにより、単純なしきい値監視では検知できない問題も拾っていくことできます。 今回は Instanaがエラー率の急増を自動的に検知して、アラートが上がっています。通常は、このようなアラートの通知をメールやSlack/Teamなどで受けて、Instanaに入ってきて解析をるすことになります。  
この 問題の通知からどのように問題切り分けをしていくか確認してみましょう。<img width="1919" alt="image" src="https://github.com/iwashinat/InstanaLab/assets/22209835/508694d5-02e7-4775-b176-50d5807981ce">
1. 問題の通知を開いてみましょう。特定の時間でエラーが出ていたことが分かります。ここにフォーカスを当てて、マークをつけておきましょう。<img width="960" alt="image" src="https://github.com/iwashinat/InstanaLab/assets/22209835/b52931bf-f157-4b94-aa8a-66063b2cdc5c">
1. 左のメニューから **イベント＊＊のビューを開きます。すべてを開いてみましょう。
Instanaが検知したプロセスの 生死のイベントや、アプリケーションのリリースなど、関連する基盤イベントと含めて、表示されています。ここでは 当該時間近くで Db2のオフラインが検知されていますね。
このオフライン・イベントを開いていきます。<img width="958" alt="image" src="https://github.com/iwashinat/InstanaLab/assets/22209835/b3c3085f-e40e-4aec-a9f3-fa14ca5b5c75">
1. イベントには、オフライン・イベントが発生した データベースの技術スタックも含まれています。今回は外部から再起動しましたが、通常はさらにここから基盤メトリックの解析を行って、問題の確認を実施していきます。ここでは一番上のDb2インスタンスのメトリックを開いてみましょう<img width="1908" alt="image" src="https://github.com/iwashinat/InstanaLab/assets/22209835/4229bf93-b189-4906-9f6d-db2e0f43fff4">
1. 以下のように 先ほどの強調表示が残っていますので、基盤メトリックと問題発生時の関連付けを考えるさいに有効です。<img width="962" alt="image" src="https://github.com/iwashinat/InstanaLab/assets/22209835/94608071-2a44-4288-ae4a-d89559b3e1e8">
1. 調査でなにか気づきがあったら、その気づきが含まれる画面をそのまま リンクで チーム・メンバーに送ることが可能です。右上にある リンクからショートURLを取得します。Instanaはすべての画面にこのShortURLのリンクがありますので、適宜Slackやメールで連携するなど、情報の共有に活用できます。<img width="959" alt="image" src="https://github.com/iwashinat/InstanaLab/assets/22209835/7adeadab-fba3-4e62-adb5-adb0fdebd484">
 
___
以上で、 Instanaのサンドボックを利用したハンズオンは終了です。お疲れさまでした。  
  
いかがでしたでしょうか？従来の基盤的なモニタリングと違い、実際の問題判別やアプリケーションの改善に繋げられるデータが多く可視化され、そして有機的に連携されていたのではないでしょうか？  
さらに一歩検証を進めて、ご自身のアプリケーション環境がどのように見えるかを試したい場合、[こちらのサイト](https://www.instana.com/getting-started-with-apm/)の**Free Trial**ボタンから２週間の無償PoCを申し込むことが可能です。ぜひ、試してみてください。

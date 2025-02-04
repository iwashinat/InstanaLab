## Applications

次に 各ホスト・ノードや Kubernertes 環境で稼働する アプリケーションについて、見ていきます。
Instanaの右のメニューから、**アプリケーション**のアイコンをクリックしてください。
![image](https://user-images.githubusercontent.com/22209835/114327991-3efac480-9b76-11eb-98a9-5bd536f758ac.png)

---
### 分散トレーシングによるアプリケーションの可視化

1. **アプリケーション**と **サービスs**の２つのタブがありますが、まずは**サービス**から見ていきます。  
ここには、Instanaエージェントが検知したアプリケーション・サービスがリストされています。  
各行に、個々のサービスのテクノロジーやそのタイプ（HTTPやDATABASE, MESSAGINGなど）、および 各サービスのゴールデン・シグナル（コール数、応答性能、エラー率）が表示されています。　従来型の環境では、各ノードのどのアプリケーション・サーバーで処理が行われているかを意識して運用をしますので、Instanaにおいても 各プロセスごとに サービス名をつけることが可能です。  <img width="1680" alt="image" src="https://github.com/iwashinat/InstanaLab/assets/22209835/04f51f3b-aa9f-4946-b269-b46b03239bd2">

1. これらのゴールデン・シグナルに対して、機械学習を適用していますので、コール数の急減や応答性能の遅延、エラー率の急増といったイベントを検知して、ヘルス状況を表示しています。  <img width="1676" alt="image" src="https://github.com/iwashinat/InstanaLab/assets/22209835/2a709053-90aa-4471-8f56-eb0ec0e965fa">
これらの問題の解析は、いったん後にして、もう少し画面を見ていきましょう。

1. つぎに **アプリケーション**のタブを開きましょう。
さきほどの サービスの タブは、テクノロジーのリストであり、業務ユーザーにとっては意味をなしません。検知されたテクノロジーを、ユーザーにとって意味のある体系に整理するビューが**アプリケーション**です。  
たとえば、特定の業務システムや特定の業務に特化したダッシュボードや、フロントエンド含めシステム全体のサービスを確認するビューなどです。また、DB管理者には、稼働するDatabaseすべてを含むビューが必要かもしません。  
特定のノードで稼働するアプリケーション、特定のKubernetes名前空間で稼働するアプリケーション、特定のサービスの下流にあるアプリケーションなど、このアプリケーション・タブは、ユーザーにとって管理しやすいよう、自由に定義・編集が可能です。<img width="1678" alt="image" src="https://github.com/iwashinat/InstanaLab/assets/22209835/1c8c89a4-525a-455a-bc49-57191fb12c39">

1. **ダイレクト**のアプリケーション・ビューをクリックしましょう。  
ダイレクトを構成するシステム全体の**サマリー**のダッシュボードが開きます。このダイレクト・アプリケーションは、Webサーバー（IBM HTTP Server）、フロントエンドAP（WebSphereAS)、バックエンドAP(WebSphereAS）、データベース（Db2）といった 典型的なのWeb３層構成となっています。  
一番上には、ゴールデン・シグナル（呼び出し数、応答性能、エラー率）およびその履歴が表示されています。  
下には関連する基盤で発生した基盤の問題と変更の履歴、各ゴールデンシグナルでのサービス（のランキング、および処理時間が表示されています。
下段中央の トップ・サービスでは、応答が悪化しているサービスやエラー数が多いサービスを確認できますので、特に注視が必要なものが分かります。待ち時間（応答性能）、呼び出し数、エラー率を選択し表示を切り替えてみてください。<img width="960" alt="image" src="https://github.com/iwashinat/InstanaLab/assets/22209835/b2698dcb-4c8e-43f4-a21f-01b7d9f38726">
1. 次に **依存関係**のタブを開きましょう。ここではダイレクトを構成する様々なサービスの依存関係（コンテキスト）が可視化されています。動いている一つ一つの点が サービス間の要求です。  
Instanaは、これらの要求を解析することで、依存関係をダイナミックに可視化しています。もし色が付いているサービスがあれば、それは問題が発生しているサービスです。<img width="1900" alt="image" src="https://github.com/iwashinat/InstanaLab/assets/22209835/243f0f2a-7255-489b-8ffa-c0a6afea2e6b">
1. ひとつのサービスにカーソルを合わせて見ましょう。そのサービスと依存関係のあるサービスのみにフォーカスが当たります。<img width="1899" alt="image" src="https://github.com/iwashinat/InstanaLab/assets/22209835/d71e22f3-08ef-4e85-9388-ddf5f1557169">
1. 左上の メニューを選択することで、応答性能が遅いサービスのアイコンを大きくしたり、要求数の大きいサービスを大きくしたり、わかりやすく可視化できますので、いろいろ触ってみてください。　　<img width="1899" alt="image" src="https://github.com/iwashinat/InstanaLab/assets/22209835/87764879-0bbe-46af-aa00-ca23c85ec6bd">

---
### サービスとエンドポイント
アプリケーションのビューで システムの全体像を把握しましたので、ここからは各アプリケーション・サーバー・プロセスやデータベースといったサービス、さらにそれらで処理されるHTTP要求やSQLといったエンドポイントへと、掘り下げてみます。

#### アプリケーション・サーバーのサービス
1. バックエンドAPのサービスを開いてみます。<img width="1908" alt="image" src="https://github.com/iwashinat/InstanaLab/assets/22209835/48d7009a-6241-4188-9d80-d10853b2e190">
1. このバックエンドAPサーバーにフォーカスしたゴールデン・シグナルのダッシュボードが表示されます。下段中央の**エンドポイント**には、個々のパスごとの処理数や応答性能が把握可能になっています。業務ごとにパスが設定されていることも多くあると思いますので、業務観点からの処理数の概算把握にも有用ですし、応答に時間がかかっている処理の特定にも有効です。<img width="1909" alt="image" src="https://github.com/iwashinat/InstanaLab/assets/22209835/11772d27-bb8b-4c99-8f58-d996c99dd4ae">
1. 特定のエンドポイントを開いていみましょう。ここでは POST /BankBackend/*orderlist* を開いてみます。この処理のみの、処理数、応答性能がエラー発生状況が分かります。<img width="963" alt="image" src="https://github.com/iwashinat/InstanaLab/assets/22209835/a285c960-6dcf-457a-8d26-6eb850126664">
1. さらに **フロー** のタブを開くと、この処理を呼び出しているフロントの処理、この処理から呼び出されるバックエンドの処理が分かります。各処理の要求数、応答性能なども表示されていますので、どの経路から処理が来ているのか、どこで応答時間がかかっているのか分かります。<img width="1898" alt="image" src="https://github.com/iwashinat/InstanaLab/assets/22209835/2a94ad8a-8dc8-42a9-9e04-091d144b7753">
1. ここで 上にある **スタック＊＊ を開いてみましょう。このアプリケーション・タブでは稼働するサービス名、インフラストラクチャ・タブでは、このアプリケーションが稼働する技術スタックを確認することが可能です <img width="1912" alt="image" src="https://github.com/iwashinat/InstanaLab/assets/22209835/17011c75-d7ab-464e-ae36-0b99abb01cee">
1. 下から二番目にある JVM （*bootstarp WAS90.SERV1*) を開いてみましょう。このアプリケーションが稼働するアプリケーション・サーバーのJVMの基盤メトリックが表示されます。GCの状況やスレッドの状況が分かりますので、たとえば実行時間が長いものがあれば基盤観点から、当該時間に GCで長く停止していることがなかったかなど確認できます。<img width="1901" alt="image" src="https://github.com/iwashinat/InstanaLab/assets/22209835/1e20fee0-f5f1-4654-90f0-c5be6b0be911">
1. 同じように **スタック＊＊ から、 **WebSphere** を開いてみましょう。<img width="962" alt="image" src="https://github.com/iwashinat/InstanaLab/assets/22209835/e1dc8416-69d5-44ee-bbee-572bf0476f05">
1. スレッド・プールの状況、データベースへの接続プールの状況、Webアプリケーションの状況などが把握できます。これまで基盤メトリックを確認するには、環境に入ってツールで確認したり、ログからツールやEXCELで加工したりといったことをしてきましたが、JavaもWebSphereもリアルタイムで現在のミドルウェアの状況を判断することができ、問題があった場合の原因判別を高速化することができます。<img width="1912" alt="image" src="https://github.com/iwashinat/InstanaLab/assets/22209835/72b38b59-ca80-496f-903d-0900f5c657bb">

#### データベースのサービス
1. 今度は ダイレクトの データベース (*db2*) のサービスの画面を開いてみましょう。
ここでは、エンドポイントに テーブルがリストされています。アクセスの集中しているテーブルなどが分かりますね。下段右には、実際に発行されているSQLステートメントがリストされていますので、ここでも応答性能が出ていないSQLなどを把握可能です。<img width="1917" alt="image" src="https://github.com/iwashinat/InstanaLab/assets/22209835/573051a9-37b7-4edb-86b6-b6a0d9302c8f">
1. さらに、エンドポイントから *PURCHASEORDER*を開いてみましょう。このテーブルに対して実行されているSQLが把握できます。<img width="962" alt="image" src="https://github.com/iwashinat/InstanaLab/assets/22209835/94633d87-c7ee-4aa1-ae3d-b61763d2c4c7">
1. **フロー**からは、このテーブルに対してアクセスを行っているアプリケーションの処理を特定することができます。どの業務の処理がテーブルに対してアクセスをいるか、すぐに特定できます。<img width="960" alt="image" src="https://github.com/iwashinat/InstanaLab/assets/22209835/21c0c37d-5f73-42b1-82db-d3f7261f930d">
1. **スタック＊＊から、Db2のメトリックを確認してみましょう。このアプリケーションでは SAMPLE というdb２インスタンスを利用しています。<img width="960" alt="image" src="https://github.com/iwashinat/InstanaLab/assets/22209835/26cf50fb-2887-4098-a2ac-a9ca863c6c5d">
1. Db2 は非常に多様なメトリックを取得しています。ロックの待機の解析や長時間実行している処理の有無、CPUリソース消費が激しいクエリなどが確認できます。<img width="1908" alt="image" src="https://github.com/iwashinat/InstanaLab/assets/22209835/2337cea4-265d-4fdd-9473-275b3189a75b">
---

ここまでで、分散トレーシングによってアプリケーションの稼働状況を理解するための**アプリケーション**の確認は終了です。分散トレーシングのアプリケーションの状況の把握と、基盤メトリックがシームレスに調査を進められるのが、Instanaの強みの一つです。  
次は、問題が発生した際に、どのようにみえるか確認していきましょう。

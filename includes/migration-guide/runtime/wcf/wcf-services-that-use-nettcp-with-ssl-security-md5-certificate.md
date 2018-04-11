### <a name="wcf-services-that-use-nettcp-with-ssl-security-and-md5-certificate-authentication"></a>SSL セキュリティおよび MD5 の証明書の認証で、NETTCP を使用する WCF サービス

|   |   |
|---|---|
|説明|.NET Framework 4.6 では、WCF SSL の既定のプロトコル一覧に TLS 1.1 および TLS 1.2 が追加されます。 クライアントとサーバー コンピューターの両方がある .NET Framework 4.6 以降がインストールされて、TLS 1.2 がネゴシエーションに使用されます。TLS 1.2 は MD5 証明書の認証をサポートしていません。 このため、顧客が MD5 証明書を使用する場合、WCF クライアントでは WCF サービスへ接続できません。|
|提案される解決策|次のいずれかの操作を実行することで、この問題を回避して、WCF クライアントを WCF サーバーに接続できるようになります。<ul><li>MD5 アルゴリズムを使用しないように証明書を更新します。 この解決策をお勧めします。</li><li>バインドがソース コードで動的に構成されていない場合は、TLS 1.1 または以前のバージョンのプロトコルを使用するようにアプリケーションの構成ファイルを更新します。 これにより、引き続き MD5 ハッシュ アルゴリズムによる証明書を使用することができます。</li></ul> <blockquote> [!WARNING] MD5 ハッシュ アルゴリズムによる証明書は安全でないと見なされるため、この回避策はお勧めできません。</blockquote> 次の構成ファイルでこれを行います。<pre><code class="language-xml">&lt;configuration&gt;&#13;&#10;&lt;system.serviceModel&gt;&#13;&#10;&lt;bindings&gt;&#13;&#10;&lt;netTcpBinding&gt;&#13;&#10;&lt;binding&gt;&#13;&#10;&lt;security mode= &quot;None/Transport/Message/TransportWithMessageCredential&quot; &gt;&#13;&#10;&lt;transport clientCredentialType=&quot;None/Windows/Certificate&quot;&#13;&#10;protectionLevel=&quot;None/Sign/EncryptAndSign&quot;&#13;&#10;sslProtocols=&quot;Ssl3/Tls1/Tls11&quot;&gt;&#13;&#10;&lt;/transport&gt;&#13;&#10;&lt;/security&gt;&#13;&#10;&lt;/binding&gt;&#13;&#10;&lt;/netTcpBinding&gt;&#13;&#10;&lt;/bindings&gt;&#13;&#10;&lt;/system.ServiceModel&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre><ul><li>バインディングはソース コードで動的に設定する場合は、更新、 <xref:System.ServiceModel.TcpTransportSecurity.SslProtocols?displayProperty=nameWithType> TLS 1.1 を使用するプロパティ (<xref:System.Security.Authentication.SslProtocols.Tls11?displayProperty=nameWithType>またはソース コードでプロトコルの以前のバージョン。</li></ul> <blockquote> [!WARNING] MD5 ハッシュ アルゴリズムによる証明書は安全でないと見なされるため、この回避策はお勧めできません。</blockquote> |
|スコープ|マイナー|
|Version|4.6|
|型|ランタイム|

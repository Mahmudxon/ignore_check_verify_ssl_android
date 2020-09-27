# Server's certificate is not trusted
```
javax.net.ssl.SSLHandshakeException:java.security.cert.CertPathValidatorException:
Trust anchor for certification path not found.
```
# Trust any server and  ignore check verify.

```
    private fun trustEveryone() {
        try {
            HttpsURLConnection.setDefaultHostnameVerifier { _, _ -> true }
            val context: SSLContext = SSLContext.getInstance("TLS")
            context.init(null, arrayOf<X509TrustManager>(object : X509TrustManager {
                @Throws(CertificateException::class)
                override fun checkClientTrusted(
                    chain: Array<X509Certificate?>?,
                    authType: String?
                ) {
                }

                @Throws(CertificateException::class)
                override fun checkServerTrusted(
                    chain: Array<X509Certificate?>?,
                    authType: String?
                ) {
                }

                override fun getAcceptedIssuers(): Array<X509Certificate> {
                    return arrayOf()
                }
            }), SecureRandom())
            HttpsURLConnection.setDefaultSSLSocketFactory(
                context.socketFactory
            )
        } catch (e: Exception) {
            e.printStackTrace()
        }
    }
```

# Signature {#reference_hdl_vwc_xdb .reference}

IoT Platform performs sender identity authentication for every access request. Therefore, you must include the signature information in requests, irrespective of the use of HTTP or HTTPS protocol.

## Signing method {#section_ar1_mbn_q2b .section}

When you are signing the parameters, you can view the AccessKeyId and the AccessKeySecret for your Alibaba Cloud account on the [AccessKey](https://usercenter.console.aliyun.com/#/manage/ak) page in the console, and then encrypt them in symmetry. An AccessKeyId is used to identify the visitor, and an AccessKeySecret is the secret key used to encrypt the signature string and verify the signature string on the server. You must keep your AccessKey confidential to minimize the risk of data breaches.

**Note:** We provide SDKs in multiple programming languages, such as Java, Python, and PHP. If you use these SDKs, you can skip the authentication process. See [Download SDKs](reseller.en-US/Reference/SDK reference/Download SDKs.md#).

Sign the request parameters as follows:

1.  Create a canonicalized query string.

    1.  Sort the request parameters in a lexicographical order.

        Sort the request parameters, including the [public request parameters](reseller.en-US/Reference/API reference/Common parameters.md#section_arc_2wv_ydb) \(except for Signature parameter\) and the special parameters of an interface in a lexicographical order.

        **Note:** When you make a request by using the GET method, these parameters constitute the part of the URL following `?` and are connected by `&`\).

    2.  Encode the parameter names and values.

        Encode the request parameters by using the UTF-8 character set and according to the  [RFC3986](http://tools.ietf.org/html/rfc3986?spm=a2c4g.11186623.2.4.MQB3aI) rules. The encoding rules are as follows:

        -   Do not encode the letters A-Z and a-z , digits 0-9,  and characters including hyphens `-`, underscores `_`, periods `.`, and tildes `~` .
        -   Encode other characters in the `%XY` format, with `XY` representing the hexadecimal ASCII value of the characters. For example, the half-width double quotes `"` are encoded as `%22`.
        -   Encode the extended UTF-8 characters in `%XY%ZA…` format.
        -   Encode whitespaces to `%20` instead of the plus signs `+`.
        Alternatively, you can use libraries that support the `application/x-www-form-urlencoded` MIME-type URL encoding.

        For example, you use `java.net.URLEncoder` of Java language. Firstly, use `percentEncode` to encode a string, and then replace plus signs `+` in the encoded string with `%20`, asterisks `*` with `%2A`, and `%7E` with tidles `~`. See the following example.

        ```
        private static final String ENCODING = "UTF-8";
        private static String percentEncode(String value) throws UnsupportedEncodingException {
        return value ! = null ? URLEncoder.encode(value, ENCODING).replace("+", "%20").replace("*", "%2A").replace("%7E", "~") : null;
        }
        ```

    3.  Connect the encoded parameter names and the values with equal signs `=`.
    4.  Connect all the request parameters with ampersands `&`. The order of the parameters must be the same as that in step a.
    You now have a canonicalized query string.

2.  Construct the string to be signed.

    You can use `percentEncode` to process the canonicalized query string that is created in step 1. See the following rules:

    ```
    StringToSign=
      HTTPMethod + "&" + //HTTPMethod: HTTP method used for making request, for example GET.
      percentEncode("/") + "&" + //percentEncode("/"): Encode backslash (/) to %2F.
      percentEncode(CanonicalizedQueryString) //Encode the canonicalized query string that is created in the step 1.
    ```

3.  Calculate the HMAC value.

    Calculate the HMAC-SHA1 value of the `StringToSign` created in step 2, according to the [RFC2104](https://www.ietf.org/rfc/rfc2104.txt) rules. The Java Based64 encoding method is used in the following example.

    ```
    Signature = Base64( HMAC-SHA1( AccessSecret, UTF-8-Encoding-Of(StringToSign) ) )
    ```

    **Note:** When calculating for the signature value, you use the AccessKeySecret of your account and an ampersand `&` \(the hexadecimal ASCII value of an ampersand is 38\) as the key. The hash algorithm used is SHA1.

4.  Calculate the signature value.

    Encode the preceding HMAC value into a string based on Base64 encoding rules to obtain the signature value.

5.  Add the signature in the request.

    Encode the value of Signature according to the [RFC3986](http://tools.ietf.org/html/rfc3986) rules, and add the encoded signature value in the request.


## Example 1: Parameter concatenation method {#section_z2w_vwt_q2b .section}

Take calling the API Pub as an example, and suppose your AccessKey information is `AccessKeyId=testid` and `AccessKeySecret=testsecret`.

1.  Create a canonicalized query string.

    ```
    http://iot.cn-shanghai.aliyuncs.com/?MessageContent=aGVsbG93b3JsZA%3D&Action=Pub&Timestamp=2017-10-02T09%3A39%3A41Z&SignatureVersion=1.0&ServiceCode=iot&Format=XML&Qos=0&SignatureNonce=0715a395-aedf-4a41-bab7-746b43d38d88&Version=2017-04-20&AccessKeyId=testid&SignatureMethod=HMAC-SHA1&RegionId=cn-shanghai&ProductKey=12345abcdeZ&TopicFullName=%2FproductKey%2Ftestdevice%2Fget
    ```

2.  Create a `StringToSign` based on the canonicalized query string.

    ```
    GET&%2F&AccessKeyId%3Dtestid%26Action%3DPub%26Format%3DXML%26MessageContent%3DaGVsbG93b3JsZA%253D%26ProductKey%3D12345abcdeZ%26Qos%3D0%26RegionId%3Dcn-shanghai%26ServiceCode%3Diot%26SignatureMethod%3DHMAC-SHA1%26SignatureNonce%3D0715a395-aedf-4a41-bab7-746b43d38d88%26SignatureVersion%3D1.0%26Timestamp%3D2017-10-02T09%253A39%253A41Z%26TopicFullName%3D%252FproductKey%252Ftestdevice%252Fget%26Version%3D2017-04-20
    ```

3.  Calculate the HMAC-SHA1 value.

    Because you have the `AccessKeySecret=testsecret`, the key used in RFC2104 calculation is testsecret&. The HMAC-SHA1 value is:

    ```
    Y9eWn4nF8QPh3c4zAFkM/k/u7eA=
    ```

4.  Add this value to the request URL as the Signature parameter value to produce the following request URL:

    ```
    http://iot.cn-shanghai.aliyuncs.com/?MessageContent=aGVsbG93b3JsZA%3D&Action=Pub&Timestamp=2017-10-02T09%3A39%3A41Z&SignatureVersion=1.0&ServiceCode=iot&Format=XML&Qos=0&SignatureNonce=0715a395-aedf-4a41-bab7-746b43d38d88&Version=2017-04-20&AccessKeyId=testid&Signature=Y9eWn4nF8QPh3c4zAFkM%2Fk%2Fu7eA%3D&SignatureMethod=HMAC-SHA1&RegionId=cn-shanghai&ProductKey=12345abcdeZ&TopicFullName=%2FproductKey%2Ftestdevice%2Fget
    ```


## Example 2: Java language coding {#section_r4x_z25_q2b .section}

The codes in this example do not depend on any third-party library package and can be directly used. The signing procedure is as follows:

1.  Create a canonicalized query string \(including sorting the parameters, and URL encoding the parameter names and values\).

    ```
    public static Map<String, String> splitQueryString(String url)
             throws URISyntaxException, UnsupportedEncodingException {
         URI uri = new URI(url);
         String query = uri.getQuery();
         final String[] pairs = query.split("&");
         TreeMap<String, String> queryMap = new TreeMap<String, String>();
         for (String pair : pairs) {
             final int idx = pair.indexOf("=");
             final String key = idx > 0 ? pair.substring(0, idx) : pair;
             if (! queryMap.containsKey(key)) {
                 queryMap.put(key, URLDecoder.decode(pair.substring(idx + 1),
                         CHARSET_UTF8));
             }
         }
         return queryMap;
     }
    /** Perform URL encoding on the parameter names and values**/
     public static String generate(String method, Map<String, String> parameter,
                                   String accessKeySecret) throws Exception {
         String signString = generateSignString(method, parameter);
         System.out.println("signString---" + signString);
         byte[] signBytes = hmacSHA1Signature(accessKeySecret + "&", signString);
         String signature = newStringByBase64(signBytes);
         System.out.println("signature---" + signature);
         if ("POST".equals(method))
             return signature;
         return URLEncoder.encode(signature, "UTF-8");
     }
    ```

2.  Create the StringToSign.

    ```
    public static String generateSignString(String httpMethod,
                                             Map<String, String> parameter) throws IOException {
         TreeMap<String, String> sortParameter = new TreeMap<String, String>();
         sortParameter.putAll(parameter);
         String canonicalizedQueryString = UrlUtil.generateQueryString(
                 sortParameter, true);
         if (null == httpMethod) {
             throw new RuntimeException("httpMethod can not be empty");
         }
         /** Create the StringToSign **/
         StringBuilder stringToSign = new StringBuilder();
         stringToSign.append(httpMethod).append(SEPARATOR);
         stringToSign.append(percentEncode("/")).append(SEPARATOR);
         stringToSign.append(percentEncode(canonicalizedQueryString));
         return stringToSign.toString();
     }
    ```

3.  Calculate for the HMAC value of the StringToSign.

    ```
    public static byte[] hmacSHA1Signature(String secret, String baseString)
             throws Exception {
         if (isEmpty(secret)) {
             throw new IOException("secret can not be empty");
         }
         if (isEmpty(baseString)) {
             return null;
         }
         Mac mac = Mac.getInstance("HmacSHA1");
         SecretKeySpec keySpec = new SecretKeySpec(
                 secret.getBytes(CHARSET_UTF8), ALGORITHM);
         mac.init(keySpec);
         return mac.doFinal(baseString.getBytes(CHARSET_UTF8));
     }
     private static boolean isEmpty(String str) {
        return (str == null || str.length() == 0);
     }
    ```

4.  Encode the HMAC value into a string according to the Base64 encoding rules, and then you obtain the signature value.

    ```
    public static String newStringByBase64(byte[] bytes)
             throws UnsupportedEncodingException {
         if (bytes == null || bytes.length == 0) {
             return null;
         }
         return new String(new BASE64Encoder().encode(bytes));
     }
     public static String composeStringToSign(Map<String, String> queries) {
         String[] sortedKeys = (String[]) queries.keySet()
                 .toArray(new String[0]);
         Arrays.sort(sortedKeys);
         StringBuilder canonicalizedQueryString = new StringBuilder();
         for (String key : sortedKeys) { canonicalizedQueryString.append("&").append(percentEncode(key))
                     .append("=")
                     .append(percentEncode((String) queries.get(key)));
         }
         StringBuilder stringToSign = new StringBuilder();
         stringToSign.append("GET");
         stringToSign.append("&");
         stringToSign.append(percentEncode("/"));
         stringToSign.append("&"); stringToSign.append(percentEncode(canonicalizedQueryString.toString()
                 .substring(1)));
         return stringToSign.toString();
     }
     public static String percentEncode(String value) {
         try {
             return value == null ? null : URLEncoder
                     .encode(value, CHARSET_UTF8).replace("+", "%20")
                     .replace("*", "%2A").replace("%7E", "~");
         } catch (Exception e) {
         }
         return "";
     }
     /**
      * get SignatureNonce
      ** */
     public static String getUniqueNonce() {
         UUID uuid = UUID.randomUUID();
         return uuid.toString();
     }
     /**
      * get timestamp
      **/
     public static String getISO8601Time() {
         Date nowDate = new Date();
         SimpleDateFormat df = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ss'Z'");
         df.setTimeZone(new SimpleTimeZone(0, "GMT"));
         return df.format(nowDate);
     }
    ```

5.  Add the signature in the request.

    ```
    public static String composeUrl(String endpoint, Map<String, String> queries)
             throws UnsupportedEncodingException {
         Map<String, String> mapQueries = queries;
         StringBuilder urlBuilder = new StringBuilder("");
         urlBuilder.append("http");
         urlBuilder.append("://").append(endpoint);
         if (-1 == urlBuilder.indexOf("?")) {
             urlBuilder.append("/?") ;
         }
         urlBuilder.append(concatQueryString(mapQueries));
         return urlBuilder.toString();
     }
     public static String concatQueryString(Map<String, String> parameters)
             throws UnsupportedEncodingException {
         if (null == parameters) {
             return null;
         }
         StringBuilder urlBuilder = new StringBuilder("");
         for (Map.Entry<String, String> entry : parameters.entrySet()) {
             String key = (String) entry.getKey();
             String val = (String) entry.getValue();
             urlBuilder.append(encode(key));
             if (val ! = null) {
                 urlBuilder.append("=").append(encode(val));
             }
             urlBuilder.append("&");
         }
         int strIndex = urlBuilder.length();
         if (parameters.size() > 0) {
             urlBuilder.deleteCharAt(strIndex - 1);
         }
         return urlBuilder.toString();
     }
     public static String encode(String value)
             throws UnsupportedEncodingException {
         return URLEncoder.encode(value, "UTF-8");
     }
    }
    ```



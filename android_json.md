#Android_JSON

~~~~{.java}
obj = new JSONObject();
obj2 = new JSONObject();
try {
    obj.put("metric", "android.test");
    obj.put("timestamp", System.currentTimeMillis() / 1000L);
    obj.put("value", et_value.getText());
    obj2.put("host", "test");
    obj2.put("cpu", 0);
    obj.put("tags", obj2);
    Log.v("dd", obj.toString());

} catch (JSONException e) {
    e.printStackTrace();
}
~~~~

* 먼저 JSON 메세지를 만든다.
* JSONObject의 put Method는 2개의 파라미터를 가진다.
  * 첫 번째는 name, 두 번째는 value다.
* JSON 메세지를 Value로 하기 위해서는 같은 방법으로 JSON 메세지를 만들고 Value로 사용한다.

~~~~{.java}
public String sendJSON(String jsonMSG, String serverURL){

    OutputStream os = null;
    InputStream is = null;
    ByteArrayOutputStream baos = null;
    HttpURLConnection conn = null;
    String response = "";
    URL url = null;
    
    try {
        url = new URL(serverURL);
        conn = (HttpURLConnection)url.openConnection();
        conn.setConnectTimeout(5*1000);
        conn.setReadTimeout(5*1000);
        conn.setRequestMethod("POST");
        conn.setRequestProperty("Cache-control", "no-cache");
        conn.setRequestProperty("Content-Type", "application/json");
        conn.setRequestProperty("Accept", "application/json");
        conn.setDoOutput(true);
        conn.setDoInput(true);

        os = conn.getOutputStream();
        os.write(jsonMSG.getBytes());
        os.flush();
        int responseCode = conn.getResponseCode();
        Log.v("dd", String.valueOf(responseCode));

        if(responseCode == HttpURLConnection.HTTP_OK)
        {
            is = conn.getInputStream();
            baos = new ByteArrayOutputStream();
            byte[] byteBuffer = new byte[1024];
            byte[] byteData = null;
            int nLength = 0;
            while((nLength = is.read(byteBuffer, 0, byteBuffer.length)) != -1){
                baos.write(byteBuffer, 0, nLength);
            }
            byteData = baos.toByteArray();
            response = new String(byteData);

            Log.v("dd", "DATA response = " + response);

        }


    } catch (MalformedURLException e) {
        e.printStackTrace();
    } catch (IOException e) {
        e.printStackTrace();
    }

    return response;
}
~~~~

* JSON 메세지를 serverURL로 보낸다.
* responseCode가 HTTP_OK 일 때 진행을 해야한다. (에러발생)

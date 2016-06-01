TestGithub
==========
 private void load() {
        final String url = "http://dev.vietbuzzad.net/kfcvietnam/api/getsession";
        final String cookie = getSharedPreferences("cookie_manager", MODE_PRIVATE).getString("cookie", null);
        new AsyncTask() {
            @Override
            protected Object doInBackground(Object[] params) {
                OkHttpClient client = new OkHttpClient();
                Request request;
                if (cookie == null) {
                    request = new Request.Builder()
                            .url(url)
                            .build();
                } else {
                    request = new Request.Builder()
                            .header("Set-Cookie",cookie)
                            .addHeader("Set-Cookie",cookie)
                            .url(url)
                            .build();
                }
                Response response;
                try {
                    Log.d("send", request.headers().toString());
                    Log.d("send", request.toString());
                    response = client.newCall(request).execute();
                    String reposeString = response.body().string();
                    Log.d("received", reposeString);
                    Log.e("received", response.headers().toString());
                    Headers headers = response.headers();
                    /*
                     * o day anh list ra toan bo key la set-cookie
                     * em hoi tui api coi minh lay cookie nao thi luu xuong
                     * den request sau add vao header nhu tren .
                     * doan code ben duoi la vi du luu thoi dung quan tam
                     */
                    List<String> arrayList =  headers.values("Set-Cookie");
                    if(arrayList != null && arrayList.size() > 0){
                        for(String item : arrayList){
                            Log.e("cookie", item);
                        }
                    }
                    if (headers.get("Set-Cookie") != null && headers.get("Set-Cookie").length() > 0) {
                        getSharedPreferences("cookie_manager", MODE_PRIVATE).edit().putString("cookie", headers.get
                                ("Set-Cookie")).apply();
                    }
                    Log.i("received", response.header("Set-Cookie"));
                } catch (IOException e) {
                    e.printStackTrace();
                }
                return null;
            }
        }.execute();
    }

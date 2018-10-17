### C#

```C#
private string Decompress(byte[] baseBytes)
  {
      using (var decompressedStream = new MemoryStream())
      using (var compressedStream = new MemoryStream(baseBytes))
      using (var deflateStream = new DeflateStream(compressedStream, CompressionMode.Decompress))
      {
          deflateStream.CopyTo(decompressedStream);
          decompressedStream.Position = 0;
          using (var streamReader = new StreamReader(decompressedStream))
          {
              return streamReader.ReadToEnd();
          }
      }
  }
```


### NodeJS

```javascript


const pako = require('pako')

const WebSocket = require('ws')

// url
const ws = new WebSocket('wss://real.okex.com:10441/websocket?compress=true')

ws.on('open', function open () {
 
  ws.send('{"channel":"ok_sub_futureusd_btc_depth_quarter","event":"addChannel"}')
})

ws.on('message', function incoming (data) {
  
  
  if (data instanceof String) {
    console.log(data)
  } else {
   
    try {
      console.log(pako.inflateRaw(data, {to: 'string'}))
    } catch (err) {
      console.log(err)
    }
  }
})
```


### Python

```python
def inflate(data):
    decompress = zlib.decompressobj(
            -zlib.MAX_WBITS  # see above
    )
    inflated = decompress.decompress(data)
    inflated += decompress.flush()
    return inflated
```


### Java2

```java
package cc.kamma.ws.demo;

import okhttp3.*;
import okio.ByteString;
import org.apache.commons.compress.compressors.deflate64.Deflate64CompressorInputStream;

import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.IOException;

public class Application {
    public static void main(String[] args) {
        OkHttpClient client = new OkHttpClient();

        Request request = new Request.Builder()
                .url("wss://real.okex.com:10441/websocket/okexapi?compress=true")
                .build();

        client.newWebSocket(request, new WebSocketListener() {
            @Override
            public void onOpen(WebSocket webSocket, Response response) {
             
                webSocket.send("{\"channel\":\"ok_sub_futureusd_btc_depth_quarter\",\"event\":\"addChannel\"}");
            }

            @Override
            public void onMessage(WebSocket webSocket, String text) {
                
                System.out.println(text);
            }

            @Override
            public void onMessage(WebSocket webSocket, ByteString bytes) {
                
                System.out.println(uncompress(bytes.toByteArray()));
            }
        });
    }


    
    private static String uncompress(byte[] bytes) {
        try (final ByteArrayOutputStream out = new ByteArrayOutputStream();
             final ByteArrayInputStream in = new ByteArrayInputStream(bytes);
             final Deflate64CompressorInputStream zin = new Deflate64CompressorInputStream(in)) {
            final byte[] buffer = new byte[1024];
            int offset;
            while (-1 != (offset = zin.read(buffer))) {
                out.write(buffer, 0, offset);
            }
            return out.toString();
        } catch (final IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```


### Java

```java
 public String uncompress(ByteBuf byteBuf){
        byte [] temp = new byte[byteBuf.readableBytes()];
        ByteBufInputStream bis = new ByteBufInputStream(byteBuf);
        StringBuilder appender = new StringBuilder();
        try {
            bis.read(temp);
            bis.close();
            Inflater infl = new Inflater(true);
            infl.setInput(temp,0,temp.length);
            byte [] result = new byte[1024];
            while (!infl.finished()){
                int length = infl.inflate(result);
                appender.append(new String(result,0,length,"UTF-8"));
            }
            infl.end();
        } catch (Exception e) {
            e.printStackTrace();
        }
        return appender.toString();
    }




```

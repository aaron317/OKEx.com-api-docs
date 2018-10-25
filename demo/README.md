 
### GO
``` GO
package main

import (
	"compress/flate"
	"bytes"
	"github.com/gorilla/websocket"
	"io/ioutil"
	
	"log"
	"net/url"


)

func main() {
	u := url.URL{Scheme: "wss", Host: "real.okex.com:10441", Path: "/websocket", RawQuery: "compress=true"}
	log.Printf("connecting to %s", u.String())

	c, _, err := websocket.DefaultDialer.Dial(u.String(), nil)
	if err != nil {
		log.Fatal("dial:", err)
	}
	defer c.Close()

	c.WriteMessage(websocket.TextMessage, []byte(`{"channel":"ok_sub_futureusd_btc_depth_quarter","event":"addChannel"}`))

	done := make(chan struct{})

	go func() {
		defer close(done)
		for {
			messageType, message, err := c.ReadMessage()
			switch messageType {
			case websocket.TextMessage:
				// no need uncompressed
				log.Printf("recv: %s", message)
			case websocket.BinaryMessage:
				// uncompressed
				text, err := GzipDecode(message)
				if err != nil {
					log.Println("err" , err)
				} else {
					log.Printf("recv: %s", text)
				}
			}
			if err != nil {
				log.Println("read:", err)
				return
			}

		}
	}()

	select {}

}

func GzipDecode(in []byte) ([]byte, error) {
	reader := flate.NewReader(bytes.NewReader(in))
	defer reader.Close()

	return ioutil.ReadAll(reader)

}



```
### C++
``` C++
int gzdecompress(Byte *zdata, uLong nzdata, Byte *data, uLong *ndata)
{
    int err = 0;
    z_stream d_stream = {0}; /* decompression stream */

    static char dummy_head[2] = {
        0x8 + 0x7 * 0x10,
        (((0x8 + 0x7 * 0x10) * 0x100 + 30) / 31 * 31) & 0xFF,
    };

    d_stream.zalloc = NULL;
    d_stream.zfree = NULL;
    d_stream.opaque = NULL;
    d_stream.next_in = zdata;
    d_stream.avail_in = 0;
    d_stream.next_out = data;

    
    if (inflateInit2(&d_stream, -MAX_WBITS) != Z_OK) {
        return -1;
    }

    // if(inflateInit2(&d_stream, 47) != Z_OK) return -1;

    while (d_stream.total_out < *ndata && d_stream.total_in < nzdata) {
        d_stream.avail_in = d_stream.avail_out = 1; /* force small buffers */
        if((err = inflate(&d_stream, Z_NO_FLUSH)) == Z_STREAM_END)
        break;

        if (err != Z_OK) {
            if (err == Z_DATA_ERROR) {
                d_stream.next_in = (Bytef*) dummy_head;
                d_stream.avail_in = sizeof(dummy_head);
                if((err = inflate(&d_stream, Z_NO_FLUSH)) != Z_OK) {
                    return -1;
                }
            } else {
                return -1;
            }
        }
    }

    if (inflateEnd(&d_stream)!= Z_OK)
        return -1;
    *ndata = d_stream.total_out;
    return 0;
}
```
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

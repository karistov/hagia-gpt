
![](https://github.com/karistov/hagia-gpt/blob/main/assets/hagia-logo.png?raw=true)

#### Local playground

1. Run `lc-serve deploy local api` on one terminal to expose the app as API using langchain-serve.
2. Run `python app.py` on another terminal for a local gradio playground.
3. Open `http://localhost:7860` on your browser and interact with the app.

#### Interact using cURL

(Change the URL to your own endpoint)

**PDF url**

```bash
curl -X 'POST' \
  'https://langchain-3ff4ab2c9d.wolf.jina.ai/ask_url' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "url": "https://uiic.co.in/sites/default/files/uploads/downloadcenter/Arogya%20Sanjeevani%20Policy%20CIS_2.pdf",
  "question": "What'\''s the cap on room rent?",
  "envs": {
    "OPENAI_API_KEY": "'"${OPENAI_API_KEY}"'"
    }
}'

{"result":" Room rent is subject to a maximum of INR 5,000 per day as specified in the Arogya Sanjeevani Policy [Page no. 1].","error":"","stdout":""}
```

**PDF file**

```bash
QPARAMS=$(echo -n 'input_data='$(echo -n '{"question": "What'\''s the cap on room rent?", "envs": {"OPENAI_API_KEY": "'"${OPENAI_API_KEY}"'"}}' | jq -s -R -r @uri))
curl -X 'POST' \
  'https://langchain-3ff4ab2c9d.wolf.jina.ai/ask_file?'"${QPARAMS}" \
  -H 'accept: application/json' \
  -H 'Content-Type: multipart/form-data' \
  -F 'file=@Arogya_Sanjeevani_Policy_CIS_2.pdf;type=application/pdf'

{"result":" Room rent is subject to a maximum of INR 5,000 per day as specified in the Arogya Sanjeevani Policy [Page no. 1].","error":"","stdout":""}
```

## Running on localhost

### Credits : [Adithya S](https://github.com/200901002)

1. Pull the image by entering the following command in your terminal or command prompt:

```bash
docker pull registry.hf.space/bhaskartripathi-pdfchatter:latest
```

2. Download the Universal Sentence Encoder locally to your project's root folder. This is important because otherwise, 915 MB will be downloaded at runtime everytime you run it.
3. Download the encoder using this [link](https://tfhub.dev/google/universal-sentence-encoder/4?tf-hub-format=compressed).
4. Extract the downloaded file and place it in your project's root folder as shown below:

```text
Root folder of your project
└───Universal Sentence Encoder
|   ├───assets
|   └───variables
|   └───saved_model.pb
|
└───app.py
```

5. If you have downloaded it locally, replace the code on line 68 in the API file:

```python
self.use = hub.load('https://tfhub.dev/google/universal-sentence-encoder/4')
```

with:

```python
self.use = hub.load('./Universal Sentence Encoder/')
```

6. Now, To run PDF-GPT, enter the following command:

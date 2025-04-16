# Using online serving audio inputs

Audio input is supported according to [OpenAI Audio API](https://platform.openai.com/docs/guides/audio?audio-generation-quickstart-example=audio-in).


1. In a terminal, start the server by running the following command:

    ```cmd
    vllm serve fixie-ai/ultravox-v0_5-llama-3_2-1b
    ```

2. Use the client with one of the following options:

    * For audio input inference using base64 encoding, use the following example:

        ```py
        import base64
        import requests
        from vllm.assets.audio import AudioAsset

        def encode_base64_content_from_url(content_url: str) -> str:
            """Encode a content retrieved from a remote url to base64 format."""

            with requests.get(content_url) as response:
                response.raise_for_status()
                result = base64.b64encode(response.content).decode('utf-8')

            return result

        openai_api_key = "EMPTY"
        openai_api_base = "http://localhost:8000/v1"

        client = {openai-product-title}(
            api_key=openai_api_key,
            base_url=openai_api_base,
        )

        audio_url = AudioAsset("winning_call").url
        audio_base64 = encode_base64_content_from_url(audio_url)

        chat_completion_from_base64 = client.chat.completions.create(
            messages=[{
                "role": "user",
                "content": [
                    {
                        "type": "text",
                        "text": "What's in this audio?"
                    },
                    {
                        "type": "input_audio",
                        "input_audio": {
                            "data": audio_base64,
                            "format": "wav"
                        },
                    },
                ],
            }],
            model=model,
            max_completion_tokens=64,
        )

        result = chat_completion_from_base64.choices[0].message.content
        print("Chat completion output from input audio:", result)
        ```

        !!! note
            You can use any format that librosa supports

    * For audio input inference using `audio_url`, use the following example:

        ```py
        chat_completion_from_url = client.chat.completions.create(
            messages=[{
                "role": "user",
                "content": [
                    {
                        "type": "text",
                        "text": "What's in this audio?"
                    },
                    {
                        "type": "audio_url",
                        "audio_url": {
                            "url": audio_url
                        },
                    },
                ],
            }],
            model=model,
            max_completion_tokens=64,
        )
        
        result = chat_completion_from_url.choices[0].message.content
        print("Chat completion output from audio url:", result)
        ```

3. Optional: To change the default timeout value for fetching images through HTTP URL, set the environment variable to the timeout value that you want to use. For example:

    ```cmd
    export VLLM_AUDIO_FETCH_TIMEOUT=<timeout>
    ```
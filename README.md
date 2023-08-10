#  Chat AI
The project integrates the ChatGPT model with GPT-4 and the Chat API, all while using SwiftUI for a sleek user interface.

## Usage
We're going to get our endpoint for our chat API, which we can initialize as follows:
```swift
private let endpointURL = "https://api.openai.com/v1/chat/completions"
```
and you can get the API key from [OpenAI](https://platform.openai.com/).

The function `sendMessage` takes an array of `Message` objects as input and is marked with the async keyword, indicating that it can be used with asynchronous programming.
```swift
func sendMessage(messages: [Message]) async -> OpenAIChatResponse? {
        // Map input messages to OpenAIChatMessage format
        let openAIMessages = messages.map({OpenAIChatMessage(role: $0.role, content: $0.content)})

        // Create the request body with the chosen model and formatted messages
        // model gpt-3.5.-turbo or gpt-4
        let body = OpenAIChatBody(model: "gpt-3.5-turbo", messages: openAIMessages)

        // Set headers for the API request
        let headers: HTTPHeaders = [
            "Authorization": "Bearer \(Constants.OpenAIApiKey)"
        ]

        // Send the API request and decode the response into OpenAIChatResponse
        return try? await AF.request(endpointURL, method: .post, parameters: body, encoder: .json, headers: headers).serializingDecodable(OpenAIChatResponse.self).value
}
```
You'll notice inside the function that the input `messages` are transformed into an array of `OpenAIChatMessage` objects using the map function. Each OpenAIChatMessage has a `role` and `content`, which are taken from the corresponding properties of the input `Message` objects.

The response from the API request is then decoded into an instance of `OpenAIChatResponse` using the `serializingDecodable` function provided by the Alamofire library.

[Learn how to use OpenAI in your project](https://platform.openai.com/docs/api-reference/)

## Package
The package that we used:
* **[Alamofire](https://github.com/alamofire/alamofire/)**: Networking framework

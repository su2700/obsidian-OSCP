
NetNTLMv2 不是“真正的（静态）密码哈希”，下面用中文把差别、原理、能做/不能做的事以及常见利用/防护讲清楚（含引用参考）。

1. 为什么“不是真正的哈希”

- NetNTLMv2 是一种挑战-响应（challenge‑response）认证产物：它是客户端用密码衍生出的密钥（NT hash）对服务器挑战和客户端 blob 做 HMAC‑MD5 得到的响应值，用于证明客户端知道密码，但它本身依赖于每次认证的 server challenge / client blob，所以是会变的，会话级别的产物，不是系统静态存储的密码哈希。技术细节及生成流程可见说明：先由密码生成 NT 哈希（MD4），再用这个作为 key 对 username+domain 做 HMAC‑MD5（得到 NTLMv2 hash），最后用 NTLMv2 hash 去 HMAC‑MD5(server_challenge + client_blob) 得到响应。

offsec::OSCP:1122334455667788:c585558bf95713a4546c4c54fbd8d029:0101000000000000... 这个格式一般为：Username::Domain:ServerChallenge:LMv2/NTLMv2-HMAC:Blob（hashcat / 大多数工具都支持这个格式作为输入）。要注意双冒号的地方（hashcat 对此格式是兼容的）
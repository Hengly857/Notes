## 架构
1. Handlers
角色: 处理 HTTP 请求和响应。
功能:
接收来自客户端的请求（如 REST API 请求）。
解析请求数据（如 JSON、表单数据等）。
调用相应的业务逻辑（通常是通过 managers 或 services）。
返回响应给客户端（如 JSON 响应、错误信息等）。
示例: 在 app/api/v2/handlers 目录下，您可能会看到处理特定 API 路由的类或函数，例如 fishing_tenant_api.py 中的 FishingTenantApi 类。
2. Managers
角色: 处理业务逻辑和数据交互。
功能:
负责协调不同的服务和数据访问层。
实现具体的业务逻辑，例如数据验证、数据转换等。
调用 services 进行数据存储、查询等操作。
可能会处理多个相关的操作，确保业务流程的完整性。
示例: 在 app/api/v2/managers 目录下，您可能会看到管理特定业务逻辑的类，例如 fishing_api_manager.py 中的 FishingApiManager 类。
3. Services
角色: 提供数据访问和持久化功能。
功能:
负责与数据库或其他外部系统的交互。
实现 CRUD 操作（创建、读取、更新、删除）。
处理数据的存储和检索，通常会使用 ORM 或直接的数据库查询。
可能会包含一些通用的业务逻辑，例如数据验证、格式化等。
示例: 在 app/service 目录下，您可能会看到提供数据访问功能的类，例如 data_svc.py 中的 DataService 类。
总结
Handlers: 负责接收请求和返回响应，处理 HTTP 交互。
Managers: 负责业务逻辑的实现，协调不同的服务和数据访问。
Services: 负责与数据库的交互，提供数据存储和检索功能。
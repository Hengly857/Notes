# api分权
权限划分，是指在一个系统中将不同级别的访问权限分配给不同的用户或角色，以确保每个用户只能访问他们被授权使用的资源和服务。
对于Go项目中的API进行分权有着重要的意义，它不仅有助于保护系统的安全性和隐私性，还能提高系统的可管理性和可靠性。
分权过程是 在 header请求头加入 organization_id_path 然后在api调用过程中进行验证
controllers/api/server.go

1.仿照 data_api router.api
2.在aes.db表里面插入organization_id_path

```
func HandlerFuncBefore(f func(http.ResponseWriter, *http.Request, *base.AuthInfo)) func(w http.ResponseWriter, r *http.Request) {
	return func(w http.ResponseWriter, r *http.Request) {
		authInfo := base.AuthInfo{OrganizationIdPath: []string{}}
		if r.Header.Get(base.HEADER_ORGIN_ID_KEY) != "" {
			authInfo.OrganizationIdPath = r.Header.Values(base.HEADER_ORGIN_ID_KEY)
			if len(authInfo.OrganizationIdPath) > 0 {
				orgIdWhere := []string{}
				orgIdArgs := []interface{}{}
				for _, orgId := range authInfo.OrganizationIdPath {
					orgIdArr := strings.Split(orgId, ",")
					for _, orgId := range orgIdArr {
						orgIdWhere = append(orgIdWhere, "organization_id_path like ?")
						orgIdArgs = append(orgIdArgs, "%"+orgId+"%")
					}
				}
				authInfo.OrganizationIdWhere = "( " + strings.Join(orgIdWhere, " or ") + " )"
				authInfo.OrganizationIdArgs = orgIdArgs
				authInfo.OrganizationIdAnd = " and "
			}
		}

		f(w, r, &authInfo)
	}
}
```
HandlerFuncBefore 函数是一个Go语言中的高阶函数，它接收一个接受三个参数的函数 f 作为输入，并返回一个新的HTTP处理函数。
这个新的处理函数在调用 f 之前会先进行一些预处理工作，特别是围绕认证信息（authInfo）的构建10。
HandlerFuncBefore 分析
输入参数
f: 这是一个函数类型，它接收三个参数：http.ResponseWriter, *http.Request, 和 *base.AuthInfo。
这里的 base.AuthInfo 是一个自定义结构体，用于携带与认证相关的信息，例如组织ID路径等。
返回值
返回的是一个符合 http.HandlerFunc 签名的函数，即它接收两个参数 http.ResponseWriter 和 *http.Request 并返回 nil。
这种模式允许我们将额外的逻辑包装到现有的HTTP处理器中，而不需要修改原有的处理器代码。
初始化 authInfo: 创建了一个空的 base.AuthInfo 实例。
检查请求头：从HTTP请求头中查找特定键 HEADER_ORGIN_ID_KEY 的值。如果存在该键，则继续下一步；否则，使用默认值。
解析组织ID路径：将找到的组织ID字符串分割成数组，并进一步拆分为更小的部分（如果有逗号分隔）。对于每个部分，构造SQL查询条件和参数列表。
设置 authInfo 属性：根据解析结果更新 authInfo 对象的属性，包括 OrganizationIdWhere 和 OrganizationIdArgs，
以及添加一个逻辑AND运算符 (OrganizationIdAnd) 以便后续可能的SQL查询构建。
调用原函数 f：最后，将构建好的 authInfo 结构体传递给原始函数 f，连同HTTP响应写入器和请求对象一起传递。

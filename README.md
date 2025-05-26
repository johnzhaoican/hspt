## **详细开发计划**

### **1. 项目总览**

开发一个基于 Web 的医院体检病人管理系统，核心功能包括：

- **体检病人信息管理和跟踪**
- **用户权限管理和控制**
- **数据统计和报表生成**

### **2. 修订后的技术栈要求**

- **后端**: **ThinkPHP 8.0** 框架
- **数据库**: **MySQL 5.7.44**
- **前端**: **Vue.js 3 + Element Plus**
- **开发环境**: 推荐使用 Cursor、VS Code 等 IDE

### **3. 详细实现要点**

### **3.1. 数据库设计与实现**

- **目标**: 设计合理、高效的数据库表结构。
- **工具**: MySQL Workbench 或 Navicat。
- **核心表结构 (建议)**:
- patients (患者信息表): id, name, gender, age, phone, id_card (UNIQUE), address, emergency_contact, emergency_phone, created_at, updated_at
- checkup_records (体检记录表): id, patient_id (FK), checkup_date, overall_result, suggestion, recheck_status (无需复查/待复查/已复查), created_at, updated_at
- recheck_items (复查项目表): id, checkup_record_id (FK), item_name, recheck_date, recheck_result, status (待进行/已完成/已取消), created_at, updated_at
- users (用户表): id, username (UNIQUE), password (加密), role_id (FK), name, phone, status (启用/禁用), last_login_at, created_at, updated_at
- user_roles (用户权限表/角色表): id, role_name (UNIQUE, 如 管理员/医生/护士), description

### **3.2. 后端核心功能模块开发 (ThinkPHP 8.0)**

- **代码规范**: 遵循 **PSR 标准规范**。
- **患者信息管理**:
- **添加/编辑/删除** 患者基本信息 (patients 表)。
- **查询患者**: 支持模糊查询和分页。
- **记录体检日期和结果**: 保存到 checkup_records 表，关联 patients。
- **跟踪复查状态**: 更新 checkup_records.recheck_status 并管理 recheck_items。
- **权限管理系统**:
- **用户认证**: 用户登录，生成认证令牌 (如 JWT)。
- **用户角色定义**: user_roles 表定义角色，users 表关联用户角色。
- **功能访问控制**: 根据用户角色在后端接口进行权限判断。

### **3.3. 系统要求实现**

- **系统性能**: 响应时间控制在 **2秒内**，优化 SQL 查询，使用索引。
- **安全性**:
- **数据加密**: 密码使用哈希加密存储 (如 bcrypt)。
- **防 SQL 注入**: 使用 ThinkPHP ORM 和查询构造器。
- **XSS 防护**: 对用户输入进行过滤和转义。
- **CSRF 防护**: 启用 ThinkPHP 的 CSRF 令牌。
- **认证授权**: 所有受保护接口都进行身份验证和权限检查。
- **可扩展性**: 采用模块化设计，清晰的目录结构和命名规范。

### **3.4. 后端 API 设计 (RESTful)**

- **原则**: 遵循 RESTful 规范，使用 HTTP 方法 (GET, POST, PUT, DELETE) 和合适的 URI。
- **核心接口 (示例)**:
- **用户认证授权**:
- POST /api/auth/login: 用户登录
- GET /api/auth/me: 获取当前登录用户信息
- **患者信息 CRUD**:
- GET /api/patients: 获取患者列表 (分页、搜索)
- GET /api/patients/{id}: 获取单个患者详情
- POST /api/patients: 添加新患者
- PUT /api/patients/{id}: 更新患者信息
- DELETE /api/patients/{id}: 删除患者
- **体检记录管理**:
- GET /api/patients/{patient_id}/checkups: 获取某患者体检记录
- POST /api/patients/{patient_id}/checkups: 添加体检记录
- PUT /api/checkups/{id}: 更新体检记录
- **复查提醒**:
- GET /api/rechecks: 获取待复查项目列表
- POST /api/checkups/{checkup_id}/rechecks: 添加复查项目
- PUT /api/rechecks/{id}/status: 更新复查项目状态

### **3.5. 前端界面开发 (Vue.js 3 + Element Plus)**

- **项目初始化**: 使用 Vue CLI 或 Vite 初始化 Vue 3 项目，安装 Element Plus。
- **响应式设计**: 利用 Element Plus 组件和 CSS 媒体查询适应不同设备。
- **直观的用户界面**:
- **登录页**: Element Plus 的 Form, Input 组件。
- **后台管理布局**: Element Plus 的 Container, Header, Aside, Main。
- **导航菜单**: Element Plus 的 Menu。
- **患者信息管理界面**: Element Plus 的 Table (列表、分页、搜索), Form, Input, Select, DatePicker (添加/编辑表单), Dialog, MessageBox (弹窗)。
- **体检记录管理界面**: 可在患者详情页内嵌套或作为单独模块。
- **复查管理界面**: Table 展示复查列表。
- **数据展示清晰**: 充分利用 Element Plus 的 **Table** 组件。对于报表，可考虑集成 **ECharts** 或 **AntV G2Plot** (后期实现)。

### **4. 建议的开发流程**

1. **需求分析和系统设计** (已完成初步整理)。
2. **数据库设计和实现**。
3. **后端项目初始化** (ThinkPHP 8.0)。
4. **后端基础 API 开发** (用户认证)。
5. **前端项目初始化** (Vue 3 + Element Plus)。
6. **前端基础布局与登录页**，并与后端对接。
7. **核心功能模块开发** (前后端协同，患者信息、体检记录、权限管理)。
8. **数据统计和报表** (后端接口，前端图表)。
9. **系统测试和优化**。
10. **部署和文档编写**。

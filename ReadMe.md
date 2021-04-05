```
SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for interface_info
-- ----------------------------
DROP TABLE IF EXISTS `interface_info`;
CREATE TABLE `interface_info`  (
  `api_id` int(11) NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `api_method` varchar(12) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL COMMENT 'HttpMethod：GET、PUT、POST',
  `api_path` varchar(512) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL COMMENT '拦截路径',
  `api_status` int(2) NOT NULL COMMENT '状态：0草稿，1发布，2有变更，3禁用',
  `api_comment` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL COMMENT '注释',
  `api_type` varchar(24) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL COMMENT '脚本类型：SQL、DataQL',
  `api_script` mediumtext CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL COMMENT '查询脚本：xxxxxxx',
  `api_schema` mediumtext CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL COMMENT '接口的请求/响应数据结构',
  `api_sample` mediumtext CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL COMMENT '请求/响应/请求头样本数据',
  `api_option` mediumtext CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL COMMENT '扩展配置信息',
  `api_create_time` datetime(0) NULL DEFAULT CURRENT_TIMESTAMP(0) COMMENT '创建时间',
  `api_gmt_time` datetime(0) NULL DEFAULT CURRENT_TIMESTAMP(0) COMMENT '修改时间',
  PRIMARY KEY (`api_id`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 4 CHARACTER SET = utf8mb4 COLLATE = utf8mb4_general_ci COMMENT = 'Dataway 中的API' ROW_FORMAT = Compact;

-- ----------------------------
-- Records of interface_info
-- ----------------------------
INSERT INTO `interface_info` VALUES (1, 'POST', '/api/selAllInterfaceInfo', 1, '', 'SQL', 'select * from interface_info;', '{}', '{\"requestBody\":\"{\\\"message\\\":\\\"Hello DataQL.\\\"}\",\"headerData\":[]}', '{\"resultStructure\":true,\"responseFormat\":\"{\\n  \\\"success\\\"      : \\\"@resultStatus\\\",\\n  \\\"message\\\"      : \\\"@resultMessage\\\",\\n  \\\"code\\\"         : \\\"@resultCode\\\",\\n  \\\"lifeCycleTime\\\": \\\"@timeLifeCycle\\\",\\n  \\\"executionTime\\\": \\\"@timeExecution\\\",\\n  \\\"value\\\"        : \\\"@resultData\\\"\\n}\"}', '2021-04-05 16:32:50', '2021-04-05 16:35:37');
INSERT INTO `interface_info` VALUES (2, 'GET', '/api/finall', 1, '', 'SQL', 'select count(1) as cc from interface_info;', '{}', '{\"requestBody\":\"{\\\"message\\\":\\\"Hello DataQL.\\\"}\",\"headerData\":[]}', '{\"resultStructure\":true,\"responseFormat\":\"{\\n  \\\"success\\\"      : \\\"@resultStatus\\\",\\n  \\\"message\\\"      : \\\"@resultMessage\\\",\\n  \\\"code\\\"         : \\\"@resultCode\\\",\\n  \\\"lifeCycleTime\\\": \\\"@timeLifeCycle\\\",\\n  \\\"executionTime\\\": \\\"@timeExecution\\\",\\n  \\\"value\\\"        : \\\"@resultData\\\"\\n}\"}', '2021-04-05 16:36:35', '2021-04-05 16:38:38');
INSERT INTO `interface_info` VALUES (3, 'GET', '/api/findAll2', 1, '自定义返回值的查询', 'DataQL', 'var queryUser = @@sql()<% \n    select * from user\n%>\n\nreturn queryUser()=>[\n    {\n        \"userId\":id,\n        \"userName\": name,\n        \"userAge\": age\n    }\n];', '{}', '{\"requestBody\":\"{\\\"userName\\\": \\\"小芳\\\"}\",\"headerData\":[]}', '{\"resultStructure\":false,\"responseFormat\":\"{\\n  \\\"success\\\": \\\"@resultStatus\\\",\\n  \\\"message\\\": \\\"@resultMessage\\\",\\n  \\\"code\\\": \\\"@resultCode\\\",\\n  \\\"value\\\": \\\"@resultData\\\"\\n}\"}', '2021-04-05 17:05:14', '2021-04-05 17:06:23');

-- ----------------------------
-- Table structure for interface_release
-- ----------------------------
DROP TABLE IF EXISTS `interface_release`;
CREATE TABLE `interface_release`  (
  `pub_id` int(11) NOT NULL AUTO_INCREMENT COMMENT 'Publish ID',
  `pub_api_id` int(11) NOT NULL COMMENT '所属API ID',
  `pub_method` varchar(12) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL COMMENT 'HttpMethod：GET、PUT、POST',
  `pub_path` varchar(512) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL COMMENT '拦截路径',
  `pub_status` int(2) NOT NULL COMMENT '状态：0有效，1无效（可能被下线）',
  `pub_type` varchar(24) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL COMMENT '脚本类型：SQL、DataQL',
  `pub_script` mediumtext CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL COMMENT '查询脚本：xxxxxxx',
  `pub_script_ori` mediumtext CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL COMMENT '原始查询脚本，仅当类型为SQL时不同',
  `pub_schema` mediumtext CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL COMMENT '接口的请求/响应数据结构',
  `pub_sample` mediumtext CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL COMMENT '请求/响应/请求头样本数据',
  `pub_option` mediumtext CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL COMMENT '扩展配置信息',
  `pub_release_time` datetime(0) NULL DEFAULT CURRENT_TIMESTAMP(0) COMMENT '发布时间（下线不更新）',
  PRIMARY KEY (`pub_id`) USING BTREE,
  INDEX `idx_interface_release`(`pub_api_id`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 8 CHARACTER SET = utf8mb4 COLLATE = utf8mb4_general_ci COMMENT = 'Dataway API 发布历史。' ROW_FORMAT = Compact;

-- ----------------------------
-- Records of interface_release
-- ----------------------------
INSERT INTO `interface_release` VALUES (1, 1, 'POST', '/api/selAllInterfaceInfo', 1, 'SQL', 'var tempCall = @@sql(`message`)<%select * from interface_info;%>;\nreturn tempCall(${message});', 'select * from interface_info;', '{}', '{\"requestBody\":\"{\\\"message\\\":\\\"Hello DataQL.\\\"}\",\"headerData\":[]}', '{\"resultStructure\":true,\"responseFormat\":\"{\\n  \\\"success\\\"      : \\\"@resultStatus\\\",\\n  \\\"message\\\"      : \\\"@resultMessage\\\",\\n  \\\"code\\\"         : \\\"@resultCode\\\",\\n  \\\"lifeCycleTime\\\": \\\"@timeLifeCycle\\\",\\n  \\\"executionTime\\\": \\\"@timeExecution\\\",\\n  \\\"value\\\"        : \\\"@resultData\\\"\\n}\"}', '2021-04-05 16:32:59');
INSERT INTO `interface_release` VALUES (2, 1, 'POST', '/api/selAllInterfaceInfo', 1, 'SQL', 'var tempCall = @@sql(`message`)<%select * from interface_info;%>;\nreturn tempCall(${message});', 'select * from interface_info;', '{}', '{\"requestBody\":\"{\\\"message\\\":\\\"Hello DataQL.\\\"}\",\"headerData\":[]}', '{\"resultStructure\":true,\"responseFormat\":\"{\\n  \\\"success\\\"      : \\\"@resultStatus\\\",\\n  \\\"message\\\"      : \\\"@resultMessage\\\",\\n  \\\"code\\\"         : \\\"@resultCode\\\",\\n  \\\"lifeCycleTime\\\": \\\"@timeLifeCycle\\\",\\n  \\\"executionTime\\\": \\\"@timeExecution\\\",\\n  \\\"value\\\"        : \\\"@resultData\\\"\\n}\"}', '2021-04-05 16:34:49');
INSERT INTO `interface_release` VALUES (3, 1, 'POST', '/api/selAllInterfaceInfo', 0, 'SQL', 'var tempCall = @@sql(`message`)<%select * from interface_info;%>;\nreturn tempCall(${message});', 'select * from interface_info;', '{}', '{\"requestBody\":\"{\\\"message\\\":\\\"Hello DataQL.\\\"}\",\"headerData\":[]}', '{\"resultStructure\":true,\"responseFormat\":\"{\\n  \\\"success\\\"      : \\\"@resultStatus\\\",\\n  \\\"message\\\"      : \\\"@resultMessage\\\",\\n  \\\"code\\\"         : \\\"@resultCode\\\",\\n  \\\"lifeCycleTime\\\": \\\"@timeLifeCycle\\\",\\n  \\\"executionTime\\\": \\\"@timeExecution\\\",\\n  \\\"value\\\"        : \\\"@resultData\\\"\\n}\"}', '2021-04-05 16:35:37');
INSERT INTO `interface_release` VALUES (4, 2, 'GET', '/api/finall', 0, 'SQL', 'var tempCall = @@sql(`message`)<%select count(1) from interface_info;%>;\nreturn tempCall(${message});', 'select count(1) from interface_info;', '{}', '{\"requestBody\":\"{\\\"message\\\":\\\"Hello DataQL.\\\"}\",\"headerData\":[]}', '{\"resultStructure\":true,\"responseFormat\":\"{\\n  \\\"success\\\"      : \\\"@resultStatus\\\",\\n  \\\"message\\\"      : \\\"@resultMessage\\\",\\n  \\\"code\\\"         : \\\"@resultCode\\\",\\n  \\\"lifeCycleTime\\\": \\\"@timeLifeCycle\\\",\\n  \\\"executionTime\\\": \\\"@timeExecution\\\",\\n  \\\"value\\\"        : \\\"@resultData\\\"\\n}\"}', '2021-04-05 16:36:43');
INSERT INTO `interface_release` VALUES (5, 2, 'GET', '/api/finall', 0, 'SQL', 'var tempCall = @@sql(`message`)<%select count(1) as cc from interface_info;%>;\nreturn tempCall(${message});', 'select count(1) as cc from interface_info;', '{}', '{\"requestBody\":\"{\\\"message\\\":\\\"Hello DataQL.\\\"}\",\"headerData\":[]}', '{\"resultStructure\":true,\"responseFormat\":\"{\\n  \\\"success\\\"      : \\\"@resultStatus\\\",\\n  \\\"message\\\"      : \\\"@resultMessage\\\",\\n  \\\"code\\\"         : \\\"@resultCode\\\",\\n  \\\"lifeCycleTime\\\": \\\"@timeLifeCycle\\\",\\n  \\\"executionTime\\\": \\\"@timeExecution\\\",\\n  \\\"value\\\"        : \\\"@resultData\\\"\\n}\"}', '2021-04-05 16:38:38');
INSERT INTO `interface_release` VALUES (6, 3, 'GET', '/api/findAll2', 0, 'DataQL', 'var queryUser = @@sql()<% \n    select * from user\n%>\n\nreturn queryUser()=>[\n    {\n        \"userId\":id,\n        \"userName\": name,\n        \"userAge\": age\n    }\n];', 'var queryUser = @@sql()<% \n    select * from user\n%>\n\nreturn queryUser()=>[\n    {\n        \"userId\":id,\n        \"userName\": name,\n        \"userAge\": age\n    }\n];', '{}', '{\"requestBody\":\"{\\\"userName\\\": \\\"小芳\\\"}\",\"headerData\":[]}', '{\"resultStructure\":true,\"responseFormat\":\"{\\n  \\\"success\\\": \\\"@resultStatus\\\",\\n  \\\"message\\\": \\\"@resultMessage\\\",\\n  \\\"code\\\": \\\"@resultCode\\\",\\n  \\\"value\\\": \\\"@resultData\\\"\\n}\"}', '2021-04-05 17:05:45');
INSERT INTO `interface_release` VALUES (7, 3, 'GET', '/api/findAll2', 0, 'DataQL', 'var queryUser = @@sql()<% \n    select * from user\n%>\n\nreturn queryUser()=>[\n    {\n        \"userId\":id,\n        \"userName\": name,\n        \"userAge\": age\n    }\n];', 'var queryUser = @@sql()<% \n    select * from user\n%>\n\nreturn queryUser()=>[\n    {\n        \"userId\":id,\n        \"userName\": name,\n        \"userAge\": age\n    }\n];', '{}', '{\"requestBody\":\"{\\\"userName\\\": \\\"小芳\\\"}\",\"headerData\":[]}', '{\"resultStructure\":false,\"responseFormat\":\"{\\n  \\\"success\\\": \\\"@resultStatus\\\",\\n  \\\"message\\\": \\\"@resultMessage\\\",\\n  \\\"code\\\": \\\"@resultCode\\\",\\n  \\\"value\\\": \\\"@resultData\\\"\\n}\"}', '2021-04-05 17:06:23');

-- ----------------------------
-- Table structure for user
-- ----------------------------
DROP TABLE IF EXISTS `user`;
CREATE TABLE `user`  (
  `id` int(11) NOT NULL,
  `name` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `age` int(11) NULL DEFAULT NULL,
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Compact;

-- ----------------------------
-- Records of user
-- ----------------------------
INSERT INTO `user` VALUES (1, '小明', 12);
INSERT INTO `user` VALUES (2, '小芳', 13);

SET FOREIGN_KEY_CHECKS = 1;

```

启动后访问：http://localhost:8080/interface-ui/#/
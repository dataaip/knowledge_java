# 企业级 Java 多模块 Spring Boot 项目架构详解

## 项目概述

这是一个基于 DDD（领域驱动设计）和 Clean Architecture 的企业级多模块 Spring Boot 项目架构，采用分层架构和模块化设计，具备高内聚、低耦合、可扩展、易维护的特性。

## 完整项目结构

```
enterprise-spring-multi-module/
├── README.md
├── pom.xml
├── .gitignore
├── .editorconfig
├── .gitattributes
├── mvnw
├── mvnw.cmd
├── LICENSE
├── CHANGELOG.md
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
├── docker-compose.yml
├── Dockerfile
├── Jenkinsfile
├── sonar-project.properties
├── .github/
│   └── workflows/
│       ├── ci.yml
│       └── cd.yml
├── docs/
│   ├── architecture/
│   │   ├── architecture-diagram.png
│   │   ├── architecture.md
│   │   └── design-principles.md
│   ├── api/
│   │   ├── swagger.json
│   │   └── postman-collection.json
│   ├── deployment/
│   │   ├── deployment-guide.md
│   │   └── kubernetes/
│   │       ├── deployment.yaml
│   │       ├── service.yaml
│   │       └── ingress.yaml
│   └── database/
│       ├── er-diagram.png
│       └── migration-guide.md
├── scripts/
│   ├── build.sh
│   ├── deploy.sh
│   ├── test.sh
│   └── database/
│       ├── backup.sh
│       └── restore.sh
└── src/
    └── main/
        └── resources/
            └── application.yml

├── common/                                               # 通用基础模块
│   ├── pom.xml
│   ├── README.md
│   └── src/
│       └── main/
│           └── java/
│               └── com/
│                   └── enterprise/
│                       └── project/
│                           └── common/
│                               ├── annotation/           # 自定义注解
│                               │   ├── AccessLog.java
│                               │   ├── DataPermission.java
│                               │   ├── RateLimiter.java
│                               │   └── ValidList.java
│                               ├── aspect/              # AOP切面
│                               │   ├── AccessLogAspect.java
│                               │   ├── DataPermissionAspect.java
│                               │   ├── RateLimitAspect.java
│                               │   └── PerformanceMonitorAspect.java
│                               ├── base/                # 基础类
│                               │   ├── BaseEntity.java
│                               │   ├── BaseDTO.java
│                               │   ├── BaseQuery.java
│                               │   ├── BaseResponse.java
│                               │   ├── BaseVO.java
│                               │   ├── PageResult.java
│                               │   └── SortOrder.java
│                               ├── config/              # 通用配置
│                               │   ├── AsyncConfig.java
│                               │   ├── CacheConfig.java
│                               │   ├── JacksonConfig.java
│                               │   ├── RedisConfig.java
│                               │   ├── SwaggerConfig.java
│                               │   ├── WebMvcConfig.java
│                               │   └── ThreadPoolConfig.java
│                               ├── constant/            # 常量定义
│                               │   ├── CacheConstants.java
│                               │   ├── CommonConstants.java
│                               │   ├── RedisConstants.java
│                               │   └── ValidationConstants.java
│                               ├── enums/               # 枚举类型
│                               │   ├── BusinessType.java
│                               │   ├── DataScope.java
│                               │   ├── HttpStatus.java
│                               │   ├── OperationType.java
│                               │   └── ResponseCode.java
│                               ├── event/               # 事件发布
│                               │   ├── ApplicationEventPublisher.java
│                               │   ├── BaseApplicationEvent.java
│                               │   └── EventListener.java
│                               ├── exception/           # 异常处理
│                               │   ├── handler/
│                               │   │   └── GlobalExceptionHandler.java
│                               │   ├── BusinessException.java
│                               │   ├── SystemException.java
│                               │   ├── ValidationException.java
│                               │   └── ExceptionCode.java
│                               ├── filter/              # 过滤器
│                               │   ├── CorsFilter.java
│                               │   ├── JwtAuthenticationFilter.java
│                               │   └── RequestLogFilter.java
│                               ├── interceptor/         # 拦截器
│                               │   ├── AuthenticationInterceptor.java
│                               │   └── PermissionInterceptor.java
│                               ├── listener/            # 监听器
│                               │   └── ApplicationStartupListener.java
│                               ├── util/                # 工具类
│                               │   ├── spring/
│                               │   │   ├── SpringContextUtil.java
│                               │   │   └── SpringBeanUtil.java
│                               │   ├── crypto/
│                               │   │   ├── AESUtil.java
│                               │   │   ├── RSAUtil.java
│                               │   │   └── SHA256Util.java
│                               │   ├── date/
│                               │   │   ├── DateUtil.java
│                               │   │   └── DateTimeFormatterUtil.java
│                               │   ├── file/
│                               │   │   ├── FileUtil.java
│                               │   │   └── ExcelUtil.java
│                               │   ├── http/
│                               │   │   ├── HttpClientUtil.java
│                               │   │   └── HttpResult.java
│                               │   ├── json/
│                               │   │   ├── JsonUtil.java
│                               │   │   └── JsonPathUtil.java
│                               │   ├── security/
│                               │   │   ├── JwtUtil.java
│                               │   │   ├── PermissionUtil.java
│                               │   │   └── SecurityUtil.java
│                               │   ├── string/
│                               │   │   ├── StringUtil.java
│                               │   │   └── UUIDUtil.java
│                               │   └── validator/
│                               │       ├── ValidatorUtil.java
│                               │       └── BeanValidator.java
│                               └── wrapper/             # 包装类
│                                   ├── RequestWrapper.java
│                                   └── ResponseWrapper.java
│           └── resources/
│               ├── META-INF/
│               │   └── spring-configuration-metadata.json
│               └── validation-messages.properties

├── domain/                                              # 领域核心模块
│   ├── pom.xml
│   ├── README.md
│   └── src/
│       └── main/
│           └── java/
│               └── com/
│                   └── enterprise/
│                       └── project/
│                           └── domain/
│                               ├── model/              # 领域模型
│                               │   ├── aggregate/      # 聚合根
│                               │   │   ├── User.java
│                               │   │   ├── Role.java
│                               │   │   ├── Permission.java
│                               │   │   ├── Organization.java
│                               │   │   └── Department.java
│                               │   ├── entity/         # 实体
│                               │   │   ├── UserProfile.java
│                               │   │   ├── UserRole.java
│                               │   │   └── RolePermission.java
│                               │   ├── valueobject/    # 值对象
│                               │   │   ├── Address.java
│                               │   │   ├── ContactInfo.java
│                               │   │   └── AuditInfo.java
│                               │   └── event/          # 领域事件
│                               │       ├── UserCreatedEvent.java
│                               │       ├── UserUpdatedEvent.java
│                               │       └── UserDeletedEvent.java
│                               ├── repository/         # 仓储接口
│                               │   ├── UserRepository.java
│                               │   ├── RoleRepository.java
│                               │   ├── PermissionRepository.java
│                               │   ├── OrganizationRepository.java
│                               │   └── DepartmentRepository.java
│                               ├── service/            # 领域服务
│                               │   ├── UserService.java
│                               │   ├── RoleService.java
│                               │   ├── PermissionService.java
│                               │   ├── OrganizationService.java
│                               │   └── DepartmentService.java
│                               ├── enums/              # 领域枚举
│                               │   ├── Gender.java
│                               │   ├── Status.java
│                               │   ├── RoleType.java
│                               │   ├── PermissionType.java
│                               │   └── OrganizationType.java
│                               ├── specification/      # 规约模式
│                               │   ├── UserSpecification.java
│                               │   ├── RoleSpecification.java
│                               │   └── OrganizationSpecification.java
│                               └── factory/            # 工厂模式
│                                   ├── UserFactory.java
│                                   ├── RoleFactory.java
│                                   └── OrganizationFactory.java
│           └── resources/
│               └── domain-messages.properties

├── infrastructure/                                      # 基础设施模块
│   ├── pom.xml
│   ├── README.md
│   └── src/
│       └── main/
│           └── java/
│               └── com/
│                   └── enterprise/
│                       └── project/
│                           └── infrastructure/
│                               ├── config/             # 基础设施配置
│                               │   ├── DatabaseConfig.java
│                               │   ├── KafkaConfig.java
│                               │   ├── RabbitMQConfig.java
│                               │   ├── ElasticsearchConfig.java
│                               │   └── MinioConfig.java
│                               ├── datasource/         # 数据源
│                               │   ├── DynamicDataSource.java
│                               │   ├── DataSourceContextHolder.java
│                               │   └── DatabaseType.java
│                               ├── repository/         # 仓储实现
│                               │   ├── impl/
│                               │   │   ├── UserRepositoryImpl.java
│                               │   │   ├── RoleRepositoryImpl.java
│                               │   │   ├── PermissionRepositoryImpl.java
│                               │   │   ├── OrganizationRepositoryImpl.java
│                               │   │   └── DepartmentRepositoryImpl.java
│                               │   └── mapper/         # MyBatis Mapper
│                               │       ├── UserMapper.java
│                               │       ├── RoleMapper.java
│                               │       ├── PermissionMapper.java
│                               │       ├── OrganizationMapper.java
│                               │       └── DepartmentMapper.java
│                               ├── persistence/        # 持久化实体
│                               │   ├── UserPO.java
│                               │   ├── RolePO.java
│                               │   ├── PermissionPO.java
│                               │   ├── OrganizationPO.java
│                               │   ├── DepartmentPO.java
│                               │   └── BasePO.java
│                               ├── client/             # 外部服务客户端
│                               │   ├── http/
│                               │   │   ├── HttpClientFactory.java
│                               │   │   └── RestTemplateClient.java
│                               │   ├── feign/
│                               │   │   ├── UserFeignClient.java
│                               │   │   └── AuthFeignClient.java
│                               │   └── message/
│                               │       ├── KafkaProducer.java
│                               │       ├── KafkaConsumer.java
│                               │       └── MessageProducer.java
│                               ├── cache/              # 缓存实现
│                               │   ├── RedisCacheManager.java
│                               │   ├── LocalCacheManager.java
│                               │   └── CacheService.java
│                               ├── search/             # 搜索引擎
│                               │   ├── ElasticsearchService.java
│                               │   └── SearchRepository.java
│                               └── storage/            # 存储服务
│                                   ├── MinioService.java
│                                   ├── FileStorageService.java
│                                   └── CloudStorageService.java
│           └── resources/
│               ├── mapper/
│               │   ├── UserMapper.xml
│               │   ├── RoleMapper.xml
│               │   ├── PermissionMapper.xml
│               │   ├── OrganizationMapper.xml
│               │   └── DepartmentMapper.xml
│               └── infrastructure-messages.properties

├── application/                                         # 应用服务模块
│   ├── pom.xml
│   ├── README.md
│   └── src/
│       └── main/
│           └── java/
│               └── com/
│                   └── enterprise/
│                       └── project/
│                           └── application/
│                               ├── service/            # 应用服务
│                               │   ├── impl/
│                               │   │   ├── UserAppService.java
│                               │   │   ├── RoleAppService.java
│                               │   │   ├── PermissionAppService.java
│                               │   │   ├── OrganizationAppService.java
│                               │   │   └── DepartmentAppService.java
│                               │   └── facade/
│                               │       ├── UserManagementFacade.java
│                               │       ├── RoleManagementFacade.java
│                               │       └── OrganizationManagementFacade.java
│                               ├── dto/                # 数据传输对象
│                               │   ├── request/
│                               │   │   ├── user/
│                               │   │   │   ├── UserCreateRequest.java
│                               │   │   │   ├── UserUpdateRequest.java
│                               │   │   │   ├── UserQueryRequest.java
│                               │   │   │   └── UserLoginRequest.java
│                               │   │   ├── role/
│                               │   │   │   ├── RoleCreateRequest.java
│                               │   │   │   └── RoleUpdateRequest.java
│                               │   │   └── organization/
│                               │   │       ├── OrganizationCreateRequest.java
│                               │   │       └── OrganizationUpdateRequest.java
│                               │   └── response/
│                               │       ├── user/
│                               │       │   ├── UserResponse.java
│                               │       │   ├── UserProfileResponse.java
│                               │       │   └── UserListResponse.java
│                               │       ├── role/
│                               │       │   ├── RoleResponse.java
│                               │       │   └── RoleListResponse.java
│                               │       └── organization/
│                               │           ├── OrganizationResponse.java
│                               │           └── OrganizationListResponse.java
│                               ├── assembler/          # 装配器
│                               │   ├── UserAssembler.java
│                               │   ├── RoleAssembler.java
│                               │   ├── PermissionAssembler.java
│                               │   ├── OrganizationAssembler.java
│                               │   └── DepartmentAssembler.java
│                               └── command/            # 命令模式
│                                   ├── user/
│                                   │   ├── CreateUserCommand.java
│                                   │   ├── UpdateUserCommand.java
│                                   │   └── DeleteUserCommand.java
│                                   └── role/
│                                       ├── CreateRoleCommand.java
│                                       └── UpdateRoleCommand.java
│           └── resources/
│               └── application-messages.properties

├── interfaces/                                          # 接口层模块
│   ├── pom.xml
│   ├── README.md
│   └── src/
│       └── main/
│           └── java/
│               └── com/
│                   └── enterprise/
│                       └── project/
│                           └── interfaces/
│                               ├── controller/         # 控制器
│                               │   ├── api/
│                               │   │   ├── UserController.java
│                               │   │   ├── RoleController.java
│                               │   │   ├── PermissionController.java
│                               │   │   ├── OrganizationController.java
│                               │   │   └── DepartmentController.java
│                               │   ├── admin/
│                               │   │   ├── AdminUserController.java
│                               │   │   └── AdminRoleController.java
│                               │   └── common/
│                               │       ├── AuthController.java
│                               │       ├── FileController.java
│                               │       └── HealthController.java
│                               ├── vo/                 # 视图对象
│                               │   ├── request/
│                               │   │   ├── user/
│                               │   │   │   ├── UserLoginVO.java
│                               │   │   │   ├── UserRegisterVO.java
│                               │   │   │   └── UserUpdateVO.java
│                               │   │   └── file/
│                               │   │       └── FileUploadVO.java
│                               │   └── response/
│                               │       ├── user/
│                               │       │   ├── UserVO.java
│                               │       │   └── LoginResultVO.java
│                               │       ├── common/
│                               │       │   ├── ApiResponse.java
│                               │       │   ├── PageResultVO.java
│                               │       │   └── ErrorResultVO.java
│                               │       └── file/
│                               │           └── FileUploadResultVO.java
│                               ├── converter/          # 转换器
│                               │   ├── UserConverter.java
│                               │   ├── RoleConverter.java
│                               │   └── OrganizationConverter.java
│                               ├── facade/             # 门面模式
│                               │   └── UserFacade.java
│                               ├── interceptor/        # 接口层拦截器
│                               │   ├── ApiRateLimitInterceptor.java
│                               │   └── LoggingInterceptor.java
│                               └── filter/             # 接口层过滤器
│                                   ├── CorsFilter.java
│                                   └── RequestLoggingFilter.java
│           └── resources/
│               └── application.yml

├── batch/                                               # 批处理模块
│   ├── pom.xml
│   ├── README.md
│   └── src/
│       └── main/
│           └── java/
│               └── com/
│                   └── enterprise/
│                       └── project/
│                           └── batch/
│                               ├── job/
│                               │   ├── UserImportJob.java
│                               │   ├── DataExportJob.java
│                               │   └── StatisticsJob.java
│                               ├── processor/
│                               │   ├── UserItemProcessor.java
│                               │   └── DataItemProcessor.java
│                               ├── reader/
│                               │   ├── UserItemReader.java
│                               │   └── ExcelItemReader.java
│                               ├── writer/
│                               │   ├── UserItemWriter.java
│                               │   └── DatabaseItemWriter.java
│                               ├── listener/
│                               │   ├── JobExecutionListener.java
│                               │   └── StepExecutionListener.java
│                               └── scheduler/
│                                   ├── BatchScheduler.java
│                                   └── ScheduledTasks.java
│           └── resources/
│               ├── batch-config.xml
│               └── batch-messages.properties

├── integration-test/                                   # 集成测试模块
│   ├── pom.xml
│   ├── README.md
│   └── src/
│       ├── main/
│       │   └── resources/
│       │       ├── application-test.yml
│       │       └── data/
│       │           ├── test-data.sql
│       │           └── schema.sql
│       └── test/
│           └── java/
│               └── com/
│                   └── enterprise/
│                       └── project/
│                           └── integration/
│                               ├── controller/
│                               │   ├── UserControllerIT.java
│                               │   ├── RoleControllerIT.java
│                               │   └── AuthControllerIT.java
│                               ├── service/
│                               │   ├── UserServiceIT.java
│                               │   └── RoleServiceIT.java
│                               ├── repository/
│                               │   ├── UserRepositoryIT.java
│                               │   └── RoleRepositoryIT.java
│                               └── batch/
│                                   └── BatchJobIT.java
│           └── resources/
│               ├── application-integration-test.yml
│               └── testcontainers/
│                   └── docker-compose-test.yml

├── unit-test/                                          # 单元测试模块
│   ├── pom.xml
│   └── src/
│       └── test/
│           └── java/
│               └── com/
│                   └── enterprise/
│                       └── project/
│                           ├── common/
│                           │   └── util/
│                           │       ├── DateUtilTest.java
│                           │       └── JsonUtilTest.java
│                           ├── domain/
│                           │   └── service/
│                           │       ├── UserServiceTest.java
│                           │       └── RoleServiceTest.java
│                           └── application/
│                               └── service/
│                                   ├── UserAppServiceTest.java
│                                   └── RoleAppServiceTest.java

└── startup/                                            # 启动模块
    ├── pom.xml
    ├── README.md
    ├── Dockerfile
    └── src/
        └── main/
            ├── java/
            │   └── com/
            │       └── enterprise/
            │           └── project/
            │               └── StartupApplication.java
            └── resources/
                ├── application.yml
                ├── application-dev.yml
                ├── application-test.yml
                ├── application-uat.yml
                ├── application-prod.yml
                ├── banner.txt
                ├── logback-spring.xml
                ├── bootstrap.yml
                ├── messages.properties
                ├── static/
                │   ├── favicon.ico
                │   └── robots.txt
                └── templates/
                    ├── error/
                    │   ├── 404.html
                    │   ├── 500.html
                    │   └── error.html
                    └── index.html
```

## 核心模块详解

### 1. common 模块（通用基础模块）

#### 1.1 注解设计
```java
// common/annotation/RateLimiter.java
package com.enterprise.project.common.annotation;

import java.lang.annotation.*;
import java.util.concurrent.TimeUnit;

/**
 * 限流注解
 */
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface RateLimiter {
    
    /**
     * 限流key
     */
    String key() default "";
    
    /**
     * 限流时间窗口（秒）
     */
    int timeWindow() default 60;
    
    /**
     * 限流次数
     */
    int limit() default 100;
    
    /**
     * 时间单位
     */
    TimeUnit timeUnit() default TimeUnit.SECONDS;
    
    /**
     * 限流策略
     */
    LimitType limitType() default LimitType.CUSTOMER;
}
```

#### 1.2 异常处理体系
```java
// common/exception/BusinessException.java
package com.enterprise.project.common.exception;

import lombok.Getter;

/**
 * 业务异常类
 */
@Getter
public class BusinessException extends RuntimeException {
    
    private final String code;
    private final Object[] args;
    
    public BusinessException(String message) {
        super(message);
        this.code = ExceptionCode.BUSINESS_ERROR.getCode();
        this.args = null;
    }
    
    public BusinessException(String code, String message) {
        super(message);
        this.code = code;
        this.args = null;
    }
    
    public BusinessException(String code, String message, Object... args) {
        super(message);
        this.code = code;
        this.args = args;
    }
    
    public BusinessException(String code, String message, Throwable cause) {
        super(message, cause);
        this.code = code;
        this.args = null;
    }
}
```

#### 1.3 基础响应对象
```java
// common/base/BaseResponse.java
package com.enterprise.project.common.base;

import com.enterprise.project.common.enums.ResponseCode;
import io.swagger.annotations.ApiModel;
import io.swagger.annotations.ApiModelProperty;
import lombok.Data;

import java.io.Serializable;
import java.time.LocalDateTime;

/**
 * 统一响应结果
 */
@Data
@ApiModel("统一响应结果")
public class BaseResponse<T> implements Serializable {
    
    private static final long serialVersionUID = 1L;
    
    @ApiModelProperty("响应码")
    private String code;
    
    @ApiModelProperty("响应消息")
    private String message;
    
    @ApiModelProperty("响应数据")
    private T data;
    
    @ApiModelProperty("时间戳")
    private LocalDateTime timestamp;
    
    @ApiModelProperty("请求ID")
    private String requestId;
    
    public BaseResponse() {
        this.timestamp = LocalDateTime.now();
    }
    
    public static <T> BaseResponse<T> success() {
        return success(null);
    }
    
    public static <T> BaseResponse<T> success(T data) {
        BaseResponse<T> response = new BaseResponse<>();
        response.setCode(ResponseCode.SUCCESS.getCode());
        response.setMessage(ResponseCode.SUCCESS.getMessage());
        response.setData(data);
        return response;
    }
    
    public static <T> BaseResponse<T> error(String code, String message) {
        BaseResponse<T> response = new BaseResponse<>();
        response.setCode(code);
        response.setMessage(message);
        return response;
    }
    
    public static <T> BaseResponse<T> error(ResponseCode responseCode) {
        return error(responseCode.getCode(), responseCode.getMessage());
    }
}
```

### 2. domain 模块（领域核心模块）

#### 2.1 聚合根设计
```java
// domain/model/aggregate/User.java
package com.enterprise.project.domain.model.aggregate;

import com.enterprise.project.common.base.BaseEntity;
import com.enterprise.project.domain.enums.Status;
import com.enterprise.project.domain.model.valueobject.Address;
import com.enterprise.project.domain.model.valueobject.ContactInfo;
import com.enterprise.project.domain.model.valueobject.AuditInfo;
import lombok.Data;
import lombok.EqualsAndHashCode;
import lombok.NoArgsConstructor;

import java.time.LocalDateTime;
import java.util.List;

/**
 * 用户聚合根
 */
@Data
@EqualsAndHashCode(callSuper = true)
@NoArgsConstructor
public class User extends BaseEntity {
    
    private String username;
    private String password;
    private String nickname;
    private String email;
    private String phone;
    private Status status;
    private String avatar;
    private LocalDateTime lastLoginTime;
    private Integer loginCount;
    
    // 值对象
    private Address address;
    private ContactInfo contactInfo;
    private AuditInfo auditInfo;
    
    // 关联关系
    private List<Role> roles;
    private Organization organization;
    private Department department;
    
    /**
     * 用户登录
     */
    public void login() {
        this.lastLoginTime = LocalDateTime.now();
        this.loginCount = this.loginCount == null ? 1 : this.loginCount + 1;
        // 发布用户登录事件
        this.registerEvent(new UserLoginEvent(this.getId(), this.username));
    }
    
    /**
     * 更新密码
     */
    public void updatePassword(String newPassword) {
        this.password = newPassword;
        this.auditInfo.setUpdatedTime(LocalDateTime.now());
        // 发布密码更新事件
        this.registerEvent(new UserPasswordUpdatedEvent(this.getId(), this.username));
    }
    
    /**
     * 验证用户状态
     */
    public boolean isActive() {
        return Status.ACTIVE.equals(this.status);
    }
}
```

#### 2.2 领域服务
```java
// domain/service/UserService.java
package com.enterprise.project.domain.service;

import com.enterprise.project.domain.model.aggregate.User;
import com.enterprise.project.domain.model.entity.UserProfile;
import com.enterprise.project.domain.repository.UserRepository;
import com.enterprise.project.common.exception.BusinessException;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.util.List;
import java.util.Optional;

/**
 * 用户领域服务
 */
@Service
@RequiredArgsConstructor
public class UserService {
    
    private final UserRepository userRepository;
    
    /**
     * 创建用户
     */
    @Transactional
    public User createUser(User user) {
        // 验证用户名唯一性
        if (userRepository.existsByUsername(user.getUsername())) {
            throw new BusinessException("用户已存在");
        }
        
        // 设置默认值
        user.setStatus(Status.ACTIVE);
        user.setLoginCount(0);
        
        // 保存用户
        User savedUser = userRepository.save(user);
        
        // 发布用户创建事件
        this.publishUserCreatedEvent(savedUser);
        
        return savedUser;
    }
    
    /**
     * 根据用户名查询用户
     */
    @Transactional(readOnly = true)
    public Optional<User> findByUsername(String username) {
        return userRepository.findByUsername(username);
    }
    
    /**
     * 用户登录
     */
    @Transactional
    public User login(String username, String password) {
        User user = userRepository.findByUsername(username)
                .orElseThrow(() -> new BusinessException("用户不存在"));
        
        // 验证密码
        if (!this.validatePassword(user, password)) {
            throw new BusinessException("密码错误");
        }
        
        // 验证用户状态
        if (!user.isActive()) {
            throw new BusinessException("用户已被禁用");
        }
        
        // 执行登录操作
        user.login();
        userRepository.save(user);
        
        return user;
    }
    
    /**
     * 验证密码
     */
    private boolean validatePassword(User user, String password) {
        // 密码验证逻辑
        return user.getPassword().equals(password); // 实际应用中应使用加密验证
    }
    
    /**
     * 发布用户创建事件
     */
    private void publishUserCreatedEvent(User user) {
        // 事件发布逻辑
    }
}
```

### 3. infrastructure 模块（基础设施模块）

#### 3.1 MyBatis Mapper 实现
```java
// infrastructure/repository/mapper/UserMapper.java
package com.enterprise.project.infrastructure.repository.mapper;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.baomidou.mybatisplus.core.metadata.IPage;
import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
import com.enterprise.project.infrastructure.persistence.UserPO;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Param;
import org.apache.ibatis.annotations.Select;

import java.util.List;

/**
 * 用户Mapper接口
 */
@Mapper
public interface UserMapper extends BaseMapper<UserPO> {
    
    /**
     * 分页查询用户
     */
    @Select("<script>" +
            "SELECT * FROM sys_user WHERE 1=1 " +
            "<if test='query.username != null and query.username != \"\"'>" +
            "AND username LIKE CONCAT('%', #{query.username}, '%') " +
            "</if>" +
            "<if test='query.status != null'>" +
            "AND status = #{query.status} " +
            "</if>" +
            "ORDER BY create_time DESC" +
            "</script>")
    IPage<UserPO> selectUserPage(Page<UserPO> page, @Param("query") UserQuery query);
    
    /**
     * 根据用户名查询用户
     */
    @Select("SELECT * FROM sys_user WHERE username = #{username}")
    UserPO selectByUsername(@Param("username") String username);
    
    /**
     * 批量插入用户
     */
    int insertBatch(@Param("users") List<UserPO> users);
}
```

#### 3.2 JPA Repository 实现
```java
// infrastructure/repository/impl/UserRepositoryImpl.java
package com.enterprise.project.infrastructure.repository.impl;

import com.enterprise.project.domain.model.aggregate.User;
import com.enterprise.project.domain.repository.UserRepository;
import com.enterprise.project.infrastructure.persistence.UserPO;
import com.enterprise.project.infrastructure.repository.mapper.UserMapper;
import com.enterprise.project.application.assembler.UserAssembler;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Repository;

import java.util.List;
import java.util.Optional;
import java.util.stream.Collectors;

/**
 * 用户仓储实现
 */
@Repository
@RequiredArgsConstructor
public class UserRepositoryImpl implements UserRepository {
    
    private final UserMapper userMapper;
    private final UserAssembler userAssembler;
    
    @Override
    public User save(User user) {
        UserPO userPO = userAssembler.toPO(user);
        if (userPO.getId() == null) {
            userMapper.insert(userPO);
        } else {
            userMapper.updateById(userPO);
        }
        return userAssembler.toDomain(userPO);
    }
    
    @Override
    public Optional<User> findById(Long id) {
        return Optional.ofNullable(userMapper.selectById(id))
                .map(userAssembler::toDomain);
    }
    
    @Override
    public Optional<User> findByUsername(String username) {
        return Optional.ofNullable(userMapper.selectByUsername(username))
                .map(userAssembler::toDomain);
    }
    
    @Override
    public List<User> findAll() {
        return userMapper.selectList(null).stream()
                .map(userAssembler::toDomain)
                .collect(Collectors.toList());
    }
    
    @Override
    public void deleteById(Long id) {
        userMapper.deleteById(id);
    }
    
    @Override
    public boolean existsByUsername(String username) {
        return userMapper.selectByUsername(username) != null;
    }
}
```

### 4. application 模块（应用服务模块）

#### 4.1 应用服务实现
```java
// application/service/impl/UserAppService.java
package com.enterprise.project.application.service.impl;

import com.enterprise.project.application.assembler.UserAssembler;
import com.enterprise.project.application.dto.request.user.UserCreateRequest;
import com.enterprise.project.application.dto.request.user.UserQueryRequest;
import com.enterprise.project.application.dto.request.user.UserUpdateRequest;
import com.enterprise.project.application.dto.response.user.UserListResponse;
import com.enterprise.project.application.dto.response.user.UserResponse;
import com.enterprise.project.application.service.UserAppService;
import com.enterprise.project.common.base.PageResult;
import com.enterprise.project.domain.model.aggregate.User;
import com.enterprise.project.domain.service.UserService;
import com.enterprise.project.infrastructure.cache.CacheService;
import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.util.List;

/**
 * 用户应用服务实现
 */
@Slf4j
@Service
@RequiredArgsConstructor
public class UserAppService implements UserAppService {
    
    private final UserService userService;
    private final UserAssembler userAssembler;
    private final CacheService cacheService;
    
    @Override
    @Transactional
    public UserResponse createUser(UserCreateRequest request) {
        log.info("创建用户，请求参数: {}", request);
        
        // DTO 转换为领域对象
        User user = userAssembler.toDomain(request);
        
        // 调用领域服务
        User savedUser = userService.createUser(user);
        
        // 清除相关缓存
        cacheService.evictUserCache();
        
        log.info("用户创建成功，用户ID: {}", savedUser.getId());
        
        // 领域对象转换为响应DTO
        return userAssembler.toResponse(savedUser);
    }
    
    @Override
    @Transactional(readOnly = true)
    public UserResponse getUserById(Long id) {
        log.info("查询用户详情，用户ID: {}", id);
        
        // 从缓存获取
        String cacheKey = "user:" + id;
        UserResponse cachedResponse = cacheService.get(cacheKey, UserResponse.class);
        if (cachedResponse != null) {
            log.info("从缓存获取用户信息，用户ID: {}", id);
            return cachedResponse;
        }
        
        // 从数据库获取
        User user = userService.findById(id)
                .orElseThrow(() -> new BusinessException("用户不存在"));
        
        UserResponse response = userAssembler.toResponse(user);
        
        // 缓存结果
        cacheService.set(cacheKey, response, 3600); // 缓存1小时
        
        return response;
    }
    
    @Override
    @Transactional(readOnly = true)
    public PageResult<UserResponse> queryUsers(UserQueryRequest request) {
        log.info("分页查询用户，请求参数: {}", request);
        
        // 调用领域服务查询
        PageResult<User> pageResult = userService.queryUsers(request);
        
        // 转换为响应对象
        List<UserResponse> responses = pageResult.getData().stream()
                .map(userAssembler::toResponse)
                .collect(Collectors.toList());
        
        return new PageResult<>(responses, pageResult.getTotal(), pageResult.getPageNum(), pageResult.getPageSize());
    }
    
    @Override
    @Transactional
    public UserResponse updateUser(Long id, UserUpdateRequest request) {
        log.info("更新用户，用户ID: {}, 请求参数: {}", id, request);
        
        // 查询用户
        User user = userService.findById(id)
                .orElseThrow(() -> new BusinessException("用户不存在"));
        
        // 更新用户信息
        userAssembler.updateDomainFromRequest(user, request);
        
        // 保存更新
        User updatedUser = userService.updateUser(user);
        
        // 清除相关缓存
        cacheService.evictUserCache();
        cacheService.delete("user:" + id);
        
        log.info("用户更新成功，用户ID: {}", updatedUser.getId());
        
        return userAssembler.toResponse(updatedUser);
    }
    
    @Override
    @Transactional
    public void deleteUser(Long id) {
        log.info("删除用户，用户ID: {}", id);
        
        // 验证用户是否存在
        if (!userService.existsById(id)) {
            throw new BusinessException("用户不存在");
        }
        
        // 删除用户
        userService.deleteUser(id);
        
        // 清除相关缓存
        cacheService.evictUserCache();
        cacheService.delete("user:" + id);
        
        log.info("用户删除成功，用户ID: {}", id);
    }
}
```

### 5. interfaces 模块（接口层模块）

#### 5.1 REST Controller 实现
```java
// interfaces/controller/api/UserController.java
package com.enterprise.project.interfaces.controller.api;

import com.enterprise.project.application.dto.request.user.UserCreateRequest;
import com.enterprise.project.application.dto.request.user.UserQueryRequest;
import com.enterprise.project.application.dto.request.user.UserUpdateRequest;
import com.enterprise.project.application.dto.response.user.UserListResponse;
import com.enterprise.project.application.dto.response.user.UserResponse;
import com.enterprise.project.application.service.UserAppService;
import com.enterprise.project.common.annotation.AccessLog;
import com.enterprise.project.common.annotation.RateLimiter;
import com.enterprise.project.common.base.BaseResponse;
import com.enterprise.project.common.base.PageResult;
import io.swagger.annotations.Api;
import io.swagger.annotations.ApiOperation;
import io.swagger.annotations.ApiParam;
import lombok.RequiredArgsConstructor;
import org.springframework.security.access.prepost.PreAuthorize;
import org.springframework.validation.annotation.Validated;
import org.springframework.web.bind.annotation.*;

import javax.validation.Valid;
import javax.validation.constraints.NotNull;

/**
 * 用户管理控制器
 */
@Api(tags = "用户管理")
@RestController
@RequestMapping("/api/v1/users")
@Validated
@RequiredArgsConstructor
public class UserController {
    
    private final UserAppService userAppService;
    
    @ApiOperation("创建用户")
    @PostMapping
    @PreAuthorize("hasAuthority('user:create')")
    @AccessLog(operation = "创建用户", businessType = "CREATE")
    @RateLimiter(key = "user:create", limit = 10, timeWindow = 60)
    public BaseResponse<UserResponse> createUser(
            @ApiParam("用户创建请求") @Valid @RequestBody UserCreateRequest request) {
        UserResponse response = userAppService.createUser(request);
        return BaseResponse.success(response);
    }
    
    @ApiOperation("获取用户详情")
    @GetMapping("/{id}")
    @PreAuthorize("hasAuthority('user:read')")
    @AccessLog(operation = "查询用户详情", businessType = "QUERY")
    public BaseResponse<UserResponse> getUserById(
            @ApiParam("用户ID") @NotNull(message = "用户ID不能为空") @PathVariable Long id) {
        UserResponse response = userAppService.getUserById(id);
        return BaseResponse.success(response);
    }
    
    @ApiOperation("分页查询用户")
    @GetMapping
    @PreAuthorize("hasAuthority('user:read')")
    @AccessLog(operation = "分页查询用户", businessType = "QUERY")
    public BaseResponse<PageResult<UserResponse>> queryUsers(
            @ApiParam("用户查询请求") @Valid UserQueryRequest request) {
        PageResult<UserResponse> result = userAppService.queryUsers(request);
        return BaseResponse.success(result);
    }
    
    @ApiOperation("更新用户")
    @PutMapping("/{id}")
    @PreAuthorize("hasAuthority('user:update')")
    @AccessLog(operation = "更新用户", businessType = "UPDATE")
    @RateLimiter(key = "user:update", limit = 20, timeWindow = 60)
    public BaseResponse<UserResponse> updateUser(
            @ApiParam("用户ID") @NotNull(message = "用户ID不能为空") @PathVariable Long id,
            @ApiParam("用户更新请求") @Valid @RequestBody UserUpdateRequest request) {
        UserResponse response = userAppService.updateUser(id, request);
        return BaseResponse.success(response);
    }
    
    @ApiOperation("删除用户")
    @DeleteMapping("/{id}")
    @PreAuthorize("hasAuthority('user:delete')")
    @AccessLog(operation = "删除用户", businessType = "DELETE")
    public BaseResponse<Void> deleteUser(
            @ApiParam("用户ID") @NotNull(message = "用户ID不能为空") @PathVariable Long id) {
        userAppService.deleteUser(id);
        return BaseResponse.success();
    }
}
```

### 6. startup 模块（启动模块）

#### 6.1 启动类
```java
// startup/StartupApplication.java
package com.enterprise.project;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.cache.annotation.EnableCaching;
import org.springframework.scheduling.annotation.EnableAsync;
import org.springframework.scheduling.annotation.EnableScheduling;
import org.springframework.transaction.annotation.EnableTransactionManagement;

/**
 * Spring Boot 启动类
 */
@SpringBootApplication(
    scanBasePackages = {
        "com.enterprise.project.common",
        "com.enterprise.project.domain",
        "com.enterprise.project.infrastructure",
        "com.enterprise.project.application",
        "com.enterprise.project.interfaces",
        "com.enterprise.project.batch",
        "com.enterprise.project.startup"
    }
)
@EnableTransactionManagement
@EnableCaching
@EnableAsync
@EnableScheduling
@EnableConfigurationProperties
public class StartupApplication {
    
    public static void main(String[] args) {
        SpringApplication.run(StartupApplication.class, args);
    }
}
```

#### 6.2 配置文件
```yaml
# startup/resources/application.yml
server:
  port: 8080
  servlet:
    context-path: /api
  tomcat:
    max-threads: 200
    min-spare-threads: 10
    accept-count: 100

spring:
  application:
    name: enterprise-spring-project
  profiles:
    active: dev
  datasource:
    url: jdbc:mysql://localhost:3306/enterprise_project?useUnicode=true&characterEncoding=utf8&useSSL=false
    username: ${DB_USERNAME:root}
    password: ${DB_PASSWORD:password}
    driver-class-name: com.mysql.cj.jdbc.Driver
    hikari:
      maximum-pool-size: 20
      minimum-idle: 5
      connection-timeout: 30000
      idle-timeout: 600000
      max-lifetime: 1800000
  jpa:
    hibernate:
      ddl-auto: validate
    show-sql: false
    properties:
      hibernate:
        format_sql: true
  redis:
    host: localhost
    port: 6379
    database: 0
    timeout: 2000ms
    lettuce:
      pool:
        max-active: 20
        max-idle: 10
        min-idle: 5
        max-wait: 2000ms
  kafka:
    bootstrap-servers: localhost:9092
    producer:
      retries: 3
      batch-size: 16384
      buffer-memory: 33554432
    consumer:
      group-id: enterprise-project-group
      auto-offset-reset: latest
      enable-auto-commit: false
  servlet:
    multipart:
      max-file-size: 10MB
      max-request-size: 100MB

# 日志配置
logging:
  level:
    com.enterprise.project: INFO
    org.springframework: WARN
    org.hibernate: WARN
  pattern:
    console: "%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n"
    file: "%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n"
  file:
    path: ./logs
    name: enterprise-project.log

# MyBatis Plus 配置
mybatis-plus:
  mapper-locations: classpath*:mapper/**/*.xml
  type-aliases-package: com.enterprise.project.infrastructure.persistence
  global-config:
    db-config:
      id-type: auto
      logic-delete-value: 1
      logic-not-delete-value: 0
  configuration:
    map-underscore-to-camel-case: true
    cache-enabled: false

# Swagger 配置
springdoc:
  swagger-ui:
    path: /swagger-ui.html
    tags-sorter: alpha
    operations-sorter: alpha
  api-docs:
    path: /v3/api-docs
  group-configs:
    - group: default
      paths-to-match: /**
      packages-to-scan: com.enterprise.project.interfaces.controller

# 应用自定义配置
app:
  jwt:
    secret: ${JWT_SECRET:defaultSecretKey}
    expiration: ${JWT_EXPIRATION:86400}
  file:
    upload-path: ${FILE_UPLOAD_PATH:/tmp/uploads}
    max-size: ${FILE_MAX_SIZE:10485760} # 10MB
  rate-limit:
    enabled: true
    default-limit: 100
    default-window: 60

management:
  endpoints:
    web:
      exposure:
        include: health,info,metrics,prometheus
  endpoint:
    health:
      show-details: always
```

## 完整的 pom.xml 配置

### 根项目 pom.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.7.15</version>
        <relativePath/>
    </parent>

    <groupId>com.enterprise.project</groupId>
    <artifactId>enterprise-spring-multi-module</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <packaging>pom</packaging>

    <name>Enterprise Spring Multi Module Project</name>
    <description>Enterprise-grade multi-module Spring Boot project with DDD architecture</description>

    <modules>
        <module>common</module>
        <module>domain</module>
        <module>infrastructure</module>
        <module>application</module>
        <module>interfaces</module>
        <module>batch</module>
        <module>integration-test</module>
        <module>unit-test</module>
        <module>startup</module>
    </modules>

    <properties>
        <java.version>11</java.version>
        <maven.compiler.source>${java.version}</maven.compiler.source>
        <maven.compiler.target>${java.version}</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        
        <!-- Spring 版本 -->
        <spring.boot.version>2.7.15</spring.boot.version>
        <spring.cloud.version>2021.0.8</spring.cloud.version>
        
        <!-- 数据库相关 -->
        <mysql.version>8.0.33</mysql.version>
        <mybatis.plus.version>3.5.3.1</mybatis.plus.version>
        <druid.version>1.2.18</druid.version>
        
        <!-- 中间件 -->
        <redis.version>2.7.2</redis.version>
        <kafka.version>3.1.1</kafka.version>
        <elasticsearch.version>7.17.3</elasticsearch.version>
        
        <!-- 工具类 -->
        <lombok.version>1.18.28</lombok.version>
        <mapstruct.version>1.5.5.Final</mapstruct.version>
        <hutool.version>5.8.20</hutool.version>
        <jackson.version>2.15.2</jackson.version>
        
        <!-- 测试 -->
        <junit.version>5.9.3</junit.version>
        <mockito.version>5.4.0</mockito.version>
        <testcontainers.version>1.18.3</testcontainers.version>
        
        <!-- 文档 -->
        <springdoc.version>1.7.0</springdoc.version>
        <swagger.version>2.2.15</swagger.version>
        
        <!-- 安全 -->
        <jwt.version>0.11.5</jwt.version>
        
        <!-- 构建工具 -->
        <maven.compiler.plugin.version>3.11.0</maven.compiler.plugin.version>
        <maven.surefire.plugin.version>3.1.2</maven.surefire.plugin.version>
        <jacoco.version>0.8.10</jacoco.version>
    </properties>

    <dependencyManagement>
        <dependencies>
            <!-- Spring Cloud -->
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring.cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>

            <!-- 项目内部模块依赖 -->
            <dependency>
                <groupId>com.enterprise.project</groupId>
                <artifactId>common</artifactId>
                <version>${project.version}</version>
            </dependency>
            <dependency>
                <groupId>com.enterprise.project</groupId>
                <artifactId>domain</artifactId>
                <version>${project.version}</version>
            </dependency>
            <dependency>
                <groupId>com.enterprise.project</groupId>
                <artifactId>infrastructure</artifactId>
                <version>${project.version}</version>
            </dependency>
            <dependency>
                <groupId>com.enterprise.project</groupId>
                <artifactId>application</artifactId>
                <version>${project.version}</version>
            </dependency>
            <dependency>
                <groupId>com.enterprise.project</groupId>
                <artifactId>interfaces</artifactId>
                <version>${project.version}</version>
            </dependency>
            <dependency>
                <groupId>com.enterprise.project</groupId>
                <artifactId>batch</artifactId>
                <version>${project.version}</version>
            </dependency>
            <dependency>
                <groupId>com.enterprise.project</groupId>
                <artifactId>integration-test</artifactId>
                <version>${project.version}</version>
            </dependency>
            <dependency>
                <groupId>com.enterprise.project</groupId>
                <artifactId>startup</artifactId>
                <version>${project.version}</version>
            </dependency>

            <!-- 数据库相关 -->
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>${mysql.version}</version>
            </dependency>
            <dependency>
                <groupId>com.baomidou</groupId>
                <artifactId>mybatis-plus-boot-starter</artifactId>
                <version>${mybatis.plus.version}</version>
            </dependency>
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>druid-spring-boot-starter</artifactId>
                <version>${druid.version}</version>
            </dependency>

            <!-- 工具类 -->
            <dependency>
                <groupId>cn.hutool</groupId>
                <artifactId>hutool-all</artifactId>
                <version>${hutool.version}</version>
            </dependency>
            <dependency>
                <groupId>org.mapstruct</groupId>
                <artifactId>mapstruct</artifactId>
                <version>${mapstruct.version}</version>
            </dependency>

            <!-- 文档 -->
            <dependency>
                <groupId>org.springdoc</groupId>
                <artifactId>springdoc-openapi-ui</artifactId>
                <version>${springdoc.version}</version>
            </dependency>

            <!-- 安全 -->
            <dependency>
                <groupId>io.jsonwebtoken</groupId>
                <artifactId>jjwt-api</artifactId>
                <version>${jwt.version}</version>
            </dependency>
            <dependency>
                <groupId>io.jsonwebtoken</groupId>
                <artifactId>jjwt-impl</artifactId>
                <version>${jwt.version}</version>
            </dependency>
            <dependency>
                <groupId>io.jsonwebtoken</groupId>
                <artifactId>jjwt-jackson</artifactId>
                <version>${jwt.version}</version>
            </dependency>

            <!-- 测试 -->
            <dependency>
                <groupId>org.testcontainers</groupId>
                <artifactId>testcontainers</artifactId>
                <version>${testcontainers.version}</version>
            </dependency>
            <dependency>
                <groupId>org.testcontainers</groupId>
                <artifactId>junit-jupiter</artifactId>
                <version>${testcontainers.version}</version>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <dependencies>
        <!-- Lombok -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>${lombok.version}</version>
            <scope>provided</scope>
        </dependency>

        <!-- MapStruct -->
        <dependency>
            <groupId>org.mapstruct</groupId>
            <artifactId>mapstruct</artifactId>
        </dependency>

        <!-- 测试依赖 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <pluginManagement>
            <plugins>
                <plugin>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-maven-plugin</artifactId>
                    <configuration>
                        <excludes>
                            <exclude>
                                <groupId>org.projectlombok</groupId>
                                <artifactId>lombok</artifactId>
                            </exclude>
                        </excludes>
                    </configuration>
                </plugin>
                
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>${maven.compiler.plugin.version}</version>
                    <configuration>
                        <source>${java.version}</source>
                        <target>${java.version}</target>
                        <encoding>${project.build.sourceEncoding}</encoding>
                        <annotationProcessorPaths>
                            <path>
                                <groupId>org.projectlombok</groupId>
                                <artifactId>lombok</artifactId>
                                <version>${lombok.version}</version>
                            </path>
                            <path>
                                <groupId>org.mapstruct</groupId>
                                <artifactId>mapstruct-processor</artifactId>
                                <version>${mapstruct.version}</version>
                            </path>
                        </annotationProcessorPaths>
                    </configuration>
                </plugin>
                
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-surefire-plugin</artifactId>
                    <version>${maven.surefire.plugin.version}</version>
                </plugin>
                
                <plugin>
                    <groupId>org.jacoco</groupId>
                    <artifactId>jacoco-maven-plugin</artifactId>
                    <version>${jacoco.version}</version>
                    <executions>
                        <execution>
                            <goals>
                                <goal>prepare-agent</goal>
                            </goals>
                        </execution>
                        <execution>
                            <id>report</id>
                            <phase>test</phase>
                            <goals>
                                <goal>report</goal>
                            </goals>
                        </execution>
                    </executions>
                </plugin>
            </plugins>
        </pluginManagement>
        
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
            </plugin>
            
            <plugin>
                <groupId>org.jacoco</groupId>
                <artifactId>jacoco-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

    <profiles>
        <profile>
            <id>dev</id>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
        </profile>
        <profile>
            <id>test</id>
        </profile>
        <profile>
            <id>prod</id>
        </profile>
    </profiles>
</project>
```

## 构建和部署流程

### 1. 本地开发
```bash
# 清理并编译
./mvnw clean compile

# 运行单元测试
./mvnw test -pl unit-test

# 运行集成测试
./mvnw verify -pl integration-test

# 启动应用
./mvnw spring-boot:run -pl startup

# 打包应用
./mvnw clean package -pl startup
```

### 2. Docker 部署
```dockerfile
# Dockerfile
FROM openjdk:11-jre-slim

LABEL maintainer="enterprise-team"

# 设置时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# 创建应用目录
WORKDIR /app

# 复制jar文件
COPY startup/target/*.jar app.jar

# 暴露端口
EXPOSE 8080

# 健康检查
HEALTHCHECK --interval=30s --timeout=3s --start-period=60s --retries=3 \
  CMD curl -f http://localhost:8080/actuator/health || exit 1

# 启动命令
ENTRYPOINT ["java", "-jar", "/app/app.jar"]
```

### 3. Kubernetes 部署
```yaml
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: enterprise-spring-project
  labels:
    app: enterprise-spring-project
spec:
  replicas: 3
  selector:
    matchLabels:
      app: enterprise-spring-project
  template:
    metadata:
      labels:
        app: enterprise-spring-project
    spec:
      containers:
      - name: enterprise-spring-project
        image: enterprise/enterprise-spring-project:1.0.0
        ports:
        - containerPort: 8080
        env:
        - name: SPRING_PROFILES_ACTIVE
          value: "prod"
        - name: DB_USERNAME
          valueFrom:
            secretKeyRef:
              name: database-secret
              key: username
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: database-secret
              key: password
        resources:
          requests:
            memory: "512Mi"
            cpu: "500m"
          limits:
            memory: "1Gi"
            cpu: "1000m"
        livenessProbe:
          httpGet:
            path: /actuator/health/liveness
            port: 8080
          initialDelaySeconds: 60
          periodSeconds: 30
        readinessProbe:
          httpGet:
            path: /actuator/health/readiness
            port: 8080
          initialDelaySeconds: 30
          periodSeconds: 10
```

## 项目特色和优势

### 1. **架构先进性**
- 遵循 DDD（领域驱动设计）原则
- 采用 Clean Architecture 分层架构
- 实现 CQRS 模式（命令查询职责分离）
- 应用六边形架构（端口适配器模式）

### 2. **代码质量保证**
- 完整的单元测试和集成测试覆盖
- 代码覆盖率监控和报告
- 静态代码分析集成（SonarQube）
- 代码规范统一（CheckStyle、SpotBugs）

### 3. **运维友好性**
- 完善的监控指标（Spring Boot Actuator）
- 健康检查和探针
- 日志统一管理和分析
- 容器化部署支持

### 4. **扩展性强**
- 模块化设计，易于扩展
- 插件化架构，支持功能热插拔
- 多环境配置管理
- 微服务拆分支持

### 5. **企业级特性**
- 完善的安全认证和授权
- 数据权限控制
- 操作日志审计
- 接口限流和熔断
- 批处理作业支持

这个架构为大型企业级应用提供了完整的解决方案，具有高度的可维护性、可扩展性和稳定性，能够满足复杂业务场景的需求。

这是一个**企业级用户管理系统的完整架构模板**，主要实现了以下功能：

## 核心功能模块

### 1. **用户管理核心功能**
- ✅ 用户注册、登录、注销
- ✅ 用户信息增删改查
- ✅ 用户状态管理（启用/禁用）
- ✅ 用户权限分配
- ✅ 用户组织架构管理
- ✅ 用户角色管理

### 2. **权限管理系统**
- ✅ 基于角色的访问控制（RBAC）
- ✅ 菜单权限管理
- ✅ 操作权限控制
- ✅ 数据权限控制
- ✅ API接口权限验证

### 3. **组织架构管理**
- ✅ 部门管理
- ✅ 组织机构管理
- ✅ 用户归属关系管理
- ✅ 多层级组织结构

### 4. **安全认证体系**
- ✅ JWT Token 认证
- ✅ 密码加密存储
- ✅ 登录日志记录
- ✅ 操作行为审计
- ✅ 接口访问限流

### 5. **系统监控运维**
- ✅ 系统健康检查
- ✅ 性能监控指标
- ✅ 操作日志审计
- ✅ 异常日志收集
- ✅ 系统配置管理

## 具体业务场景

### 用户相关操作
```
用户管理功能树:
├── 用户基础信息管理
│   ├── 用户创建/注册
│   ├── 用户信息修改
│   ├── 用户状态变更
│   ├── 用户删除/注销
│   └── 用户详情查询
├── 用户认证授权
│   ├── 用户登录验证
│   ├── Token签发验证
│   ├── 权限检查
│   └── 会话管理
├── 用户组织关系
│   ├── 部门归属管理
│   ├── 角色分配
│   └── 组织架构查询
└── 用户批量操作
    ├── 批量导入
    ├── 批量导出
    └── 批量状态变更
```

### 权限相关操作
```
权限管理功能树:
├── 角色管理
│   ├── 角色创建/修改
│   ├── 角色删除
│   ├── 角色权限配置
│   └── 角色用户分配
├── 菜单管理
│   ├── 菜单创建/修改
│   ├── 菜单删除
│   └── 菜单权限配置
├── 数据权限
│   ├── 数据范围配置
│   ├── 部门数据权限
│   └── 个人数据权限
└── 接口权限
    ├── API接口管理
    ├── 权限标识配置
    └── 访问控制策略
```

## 技术实现特点

### RESTful API 接口示例
```bash
# 用户管理接口
POST   /api/v1/users           # 创建用户
GET    /api/v1/users/{id}      # 获取用户详情
GET    /api/v1/users           # 分页查询用户
PUT    /api/v1/users/{id}      # 更新用户信息
DELETE /api/v1/users/{id}      # 删除用户

# 角色管理接口
POST   /api/v1/roles           # 创建角色
GET    /api/v1/roles/{id}      # 获取角色详情
GET    /api/v1/roles           # 分页查询角色
PUT    /api/v1/roles/{id}      # 更新角色
DELETE /api/v1/roles/{id}      # 删除角色

# 登录认证接口
POST   /api/v1/auth/login      # 用户登录
POST   /api/v1/auth/logout     # 用户登出
GET    /api/v1/auth/info       # 获取当前用户信息
```

### 业务流程示例

#### 用户登录流程
```
1. 用户输入用户名密码 → 
2. 系统验证用户身份 → 
3. 生成JWT Token → 
4. 返回Token和用户信息 → 
5. 前端存储Token → 
6. 后续请求携带Token进行权限验证
```

#### 权限验证流程
```
1. 请求到达接口层 → 
2. 拦截器验证Token有效性 → 
3. 解析用户权限信息 → 
4. 检查接口所需权限 → 
5. 权限通过则执行业务逻辑 → 
6. 返回处理结果
```

## 适用业务场景

这个架构模板适用于以下类型的企业级应用：

### 1. **企业内部管理系统**
- OA办公自动化系统
- CRM客户关系管理
- ERP企业资源规划
- HRM人力资源管理

### 2. **平台管理系统**
- 电商后台管理
- 内容管理平台
- 教育培训平台
- 医疗信息管理

### 3. **多租户SaaS应用**
- 企业服务云平台
- 行业解决方案平台
- 第三方服务集成平台

## 实际部署效果

部署后可以提供：
- **Web管理后台**：管理员进行用户、角色、权限管理
- **API服务接口**：为前端应用和其他系统提供用户认证授权服务
- **移动APP支持**：移动端用户登录认证
- **第三方集成**：单点登录、OAuth2.0等

这个项目实际上是一个**企业级用户权限管理平台的基础架构**，可以作为各种需要用户管理、权限控制的业务系统的底层框架。


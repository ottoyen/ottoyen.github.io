# Banking Application 雲原生重構指南
## 適用於 Google Cloud Run 部署

### 📋 目錄
1. [現況分析](#現況分析)
2. [雲原生架構設計](#雲原生架構設計)
3. [重構策略](#重構策略)
4. [技術棧升級](#技術棧升級)
5. [容器化配置](#容器化配置)
6. [資料庫遷移](#資料庫遷移)
7. [CI/CD 流程](#cicd-流程)
8. [監控與日誌](#監控與日誌)
9. [安全性考量](#安全性考量)
10. [實施步驟](#實施步驟)

---

## 🔍 現況分析

### 目前架構特點
- **單體應用**: 所有功能在一個 WAR 檔案中
- **傳統技術**: JSP + Servlet + JDBC
- **狀態管理**: HTTP Session 依賴
- **資料庫**: 直接 MySQL 連接
- **部署方式**: Tomcat 容器

### 雲原生轉換挑戰
1. **狀態依賴**: JSP Session 不適合無狀態容器
2. **資料庫連接**: 直接 JDBC 連接不適合雲環境
3. **靜態資源**: JSP 渲染效能問題
4. **擴展性**: 單體架構難以彈性伸縮

---

## 🏗️ 雲原生架構設計

### 建議架構模式
```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   Web Frontend  │────│  API Gateway     │────│  Cloud Run      │
│   (React/Vue)   │    │  (Cloud Load     │    │  Services       │
│                 │    │   Balancer)      │    │                 │
└─────────────────┘    └──────────────────┘    └─────────────────┘
                                                         │
                       ┌─────────────────────────────────┼─────────────────┐
                       │                                 │                 │
                ┌──────▼──────┐              ┌───────────▼──────┐  ┌───────▼────────┐
                │ Auth Service │             │ Banking Service  │  │ Transaction    │
                │ (Cloud Run) │              │ (Cloud Run)      │  │ Service        │
                │             │              │                  │  │ (Cloud Run)    │
                └─────────────┘              └─────────────────┘   └────────────────┘
                       │                              │                     │
                       └──────────────┬───────────────┴─────────────────────┘
                                      │
                              ┌───────▼────────┐
                              │ Cloud SQL      │
                              │ (PostgreSQL)   │
                              └────────────────┘
```

### 微服務拆分建議

#### 1️⃣ **Authentication Service**
- 功能: 用戶認證、JWT Token 管理
- 技術: Spring Boot + Spring Security
- 部署: Cloud Run (最小資源配置)

#### 2️⃣ **Account Service**
- 功能: 帳戶管理、餘額查詢
- 技術: Spring Boot + JPA
- 部署: Cloud Run (自動擴展)

#### 3️⃣ **Transaction Service**
- 功能: 交易處理、歷史記錄
- 技術: Spring Boot + JPA + Redis
- 部署: Cloud Run (高可用性配置)

#### 4️⃣ **Frontend Application**
- 功能: 用戶界面
- 技術: React/Vue.js + Nginx
- 部署: Cloud Run (靜態內容服務)

---

## 🔄 重構策略

### Phase 1: 技術棧現代化 (4-6 週)
1. **JSP → REST API + SPA**
   - 將 JSP 頁面轉換為 REST API
   - 開發現代化前端 (React/Vue)
   - 實現 JWT 認證機制

2. **Session → Stateless**
   - 移除 HTTP Session 依賴
   - 實現 JWT Token 認證
   - 狀態存儲移至資料庫/Redis

### Phase 2: 容器化準備 (2-3 週)
1. **Spring Boot 遷移**
   - 將 Servlet 轉換為 Spring Boot Controller
   - 整合 Spring Data JPA
   - 配置 externalized configuration

2. **資料庫連接優化**
   - 實現連接池管理
   - 環境變數配置
   - Health Check 端點

### Phase 3: 微服務拆分 (6-8 週)
1. **服務拆分**
   - 按業務邏輯拆分服務
   - 實現服務間通信 (HTTP/gRPC)
   - 資料庫按服務分離

2. **API Gateway 整合**
   - 統一入口點
   - 路由與負載均衡
   - 安全策略實施

### Phase 4: 雲原生部署 (3-4 週)
1. **Cloud Run 部署**
   - 容器映像建構
   - 環境配置管理
   - 監控與日誌設定

---

## 🛠️ 技術棧升級

### 從現有技術遷移

| 現有技術 | 建議升級至 | 理由 |
|---------|-----------|------|
| JSP + Servlet | Spring Boot + REST API | 無狀態、微服務友好、雲原生支援 |
| JDBC | Spring Data JPA | 簡化資料存取、連接池管理 |
| HTTP Session | JWT + Redis | 無狀態認證、可擴展 |
| MySQL (自管) | Cloud SQL (PostgreSQL) | 受管服務、自動備份、高可用 |
| Tomcat | Cloud Run | 無伺服器、自動擴展、成本優化 |
| Maven WAR | Docker + Maven JAR | 容器化、輕量級部署 |

### 新增技術組件

#### **後端框架**
```xml
<!-- Spring Boot Starter -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>

<!-- Spring Security for JWT -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>

<!-- Spring Data JPA -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>

<!-- PostgreSQL Driver -->
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
</dependency>

<!-- Redis for Session Management -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

#### **前端技術**
```json
{
  "dependencies": {
    "react": "^18.2.0",
    "react-router-dom": "^6.8.0",
    "axios": "^1.3.0",
    "@mui/material": "^5.11.0",
    "react-query": "^3.39.0"
  }
}
```

---

## 🐳 容器化配置

### Dockerfile 範例

#### **後端服務 Dockerfile**
```dockerfile
# Multi-stage build
FROM maven:3.8.6-openjdk-11 AS build
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline -B
COPY src ./src
RUN mvn clean package -DskipTests

FROM openjdk:11-jre-slim
WORKDIR /app
COPY --from=build /app/target/banking-service.jar app.jar

# Cloud Run 要求
EXPOSE 8080
ENV PORT=8080

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD curl -f http://localhost:8080/actuator/health || exit 1

# Non-root user for security
RUN addgroup --system spring && adduser --system spring --ingroup spring
USER spring:spring

ENTRYPOINT ["java", "-jar", "/app/app.jar"]
```

#### **前端 Dockerfile**
```dockerfile
# Build stage
FROM node:18-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
RUN npm run build

# Production stage
FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/nginx.conf

EXPOSE 8080
CMD ["nginx", "-g", "daemon off;"]
```

### Cloud Run 配置

#### **service.yaml 範例**
```yaml
apiVersion: serving.knative.dev/v1
kind: Service
metadata:
  name: banking-api
  annotations:
    run.googleapis.com/ingress: all
    run.googleapis.com/execution-environment: gen2
spec:
  template:
    metadata:
      annotations:
        autoscaling.knative.dev/minScale: "1"
        autoscaling.knative.dev/maxScale: "10"
        run.googleapis.com/cpu-throttling: "false"
        run.googleapis.com/memory: "512Mi"
        run.googleapis.com/cpu: "1000m"
    spec:
      containerConcurrency: 100
      containers:
      - image: gcr.io/PROJECT-ID/banking-api:latest
        ports:
        - containerPort: 8080
        env:
        - name: SPRING_PROFILES_ACTIVE
          value: "cloud"
        - name: DB_HOST
          valueFrom:
            secretKeyRef:
              name: db-config
              key: host
        resources:
          limits:
            cpu: 1000m
            memory: 512Mi
```

---

## 🗄️ 資料庫遷移

### Cloud SQL 配置

#### **從 MySQL 到 PostgreSQL 遷移**

1. **Schema 轉換**
```sql
-- MySQL (原有)
CREATE TABLE accounts (
    account_id INT PRIMARY KEY AUTO_INCREMENT,
    account_number VARCHAR(20) UNIQUE NOT NULL,
    customer_name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password VARCHAR(100) NOT NULL,
    balance DECIMAL(10, 2) DEFAULT 0.00,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- PostgreSQL (目標)
CREATE TABLE accounts (
    account_id SERIAL PRIMARY KEY,
    account_number VARCHAR(20) UNIQUE NOT NULL,
    customer_name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    balance DECIMAL(10, 2) DEFAULT 0.00,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- 新增索引
CREATE INDEX idx_accounts_email ON accounts(email);
CREATE INDEX idx_accounts_account_number ON accounts(account_number);
```

2. **連接配置**
```yaml
# application-cloud.yml
spring:
  datasource:
    url: jdbc:postgresql://CLOUD_SQL_CONNECTION_NAME/banking_db
    username: ${DB_USER}
    password: ${DB_PASSWORD}
    driver-class-name: org.postgresql.Driver
  
  jpa:
    database-platform: org.hibernate.dialect.PostgreSQLDialect
    hibernate:
      ddl-auto: validate
    show-sql: false
  
  # Cloud SQL Proxy configuration
  cloud:
    gcp:
      sql:
        instance-connection-name: PROJECT_ID:REGION:INSTANCE_NAME
```

### Redis 配置 (Session Store)
```yaml
spring:
  redis:
    url: redis://REDIS_HOST:6379
    timeout: 2000ms
    jedis:
      pool:
        max-active: 8
        max-idle: 8
        min-idle: 0
```

---

## 🚀 CI/CD 流程

### GitHub Actions 配置

#### **.github/workflows/deploy.yml**
```yaml
name: Deploy to Cloud Run

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

env:
  PROJECT_ID: your-gcp-project-id
  REGION: asia-northeast1
  SERVICE_NAME: banking-api

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    
    - name: Set up JDK 11
      uses: actions/setup-java@v3
      with:
        java-version: '11'
        distribution: 'temurin'
    
    - name: Cache Maven packages
      uses: actions/cache@v3
      with:
        path: ~/.m2
        key: ${{ runner.os }}-m2-${{ hashFiles('**/pom.xml') }}
    
    - name: Run tests
      run: mvn clean test

  deploy:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    steps:
    - uses: actions/checkout@v3
    
    - id: 'auth'
      uses: 'google-github-actions/auth@v1'
      with:
        credentials_json: '${{ secrets.GCP_SA_KEY }}'
    
    - name: 'Set up Cloud SDK'
      uses: 'google-github-actions/setup-gcloud@v1'
    
    - name: Configure Docker
      run: gcloud auth configure-docker
    
    - name: Build and Push Docker image
      run: |
        docker build -t gcr.io/$PROJECT_ID/$SERVICE_NAME:$GITHUB_SHA .
        docker push gcr.io/$PROJECT_ID/$SERVICE_NAME:$GITHUB_SHA
    
    - name: Deploy to Cloud Run
      run: |
        gcloud run deploy $SERVICE_NAME \
          --image gcr.io/$PROJECT_ID/$SERVICE_NAME:$GITHUB_SHA \
          --platform managed \
          --region $REGION \
          --allow-unauthenticated \
          --set-env-vars="SPRING_PROFILES_ACTIVE=cloud"
```

---

## 📊 監控與日誌

### Google Cloud 整合

#### **應用程式監控配置**
```yaml
# application.yml
management:
  endpoints:
    web:
      exposure:
        include: health,info,prometheus,metrics
  endpoint:
    health:
      show-details: always
  metrics:
    export:
      stackdriver:
        enabled: true
        project-id: ${GCP_PROJECT_ID}

logging:
  level:
    com.banking: INFO
    org.springframework.security: DEBUG
  pattern:
    console: "%d{yyyy-MM-dd HH:mm:ss} - %msg%n"
    file: "%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n"
```

#### **自訂 Metrics**
```java
@Component
public class BankingMetrics {
    
    private final Counter transactionCounter;
    private final Timer transactionTimer;
    
    public BankingMetrics(MeterRegistry meterRegistry) {
        this.transactionCounter = Counter.builder("banking.transactions.total")
            .description("Total number of transactions")
            .tag("type", "all")
            .register(meterRegistry);
            
        this.transactionTimer = Timer.builder("banking.transaction.duration")
            .description("Transaction processing time")
            .register(meterRegistry);
    }
    
    public void recordTransaction(String type) {
        transactionCounter.increment(Tags.of("type", type));
    }
}
```

### 日誌結構化
```java
// 使用 Logback with JSON formatting
@Slf4j
@RestController
public class TransactionController {
    
    @PostMapping("/transactions")
    public ResponseEntity<Transaction> createTransaction(@RequestBody TransactionRequest request) {
        log.info("Processing transaction request: accountId={}, amount={}, type={}", 
                request.getAccountId(), request.getAmount(), request.getType());
        
        try {
            Transaction result = transactionService.process(request);
            log.info("Transaction completed successfully: transactionId={}", result.getId());
            return ResponseEntity.ok(result);
        } catch (Exception e) {
            log.error("Transaction failed: accountId={}, error={}", 
                     request.getAccountId(), e.getMessage(), e);
            throw e;
        }
    }
}
```

---

## 🔒 安全性考量

### 1. **認證與授權**
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .csrf().disable()
            .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)
            .authorizeHttpRequests(authz -> authz
                .requestMatchers("/api/auth/**").permitAll()
                .requestMatchers("/actuator/health").permitAll()
                .anyRequest().authenticated()
            )
            .oauth2ResourceServer().jwt();
        
        return http.build();
    }
}
```

### 2. **資料加密**
```java
@Entity
public class Account {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false)
    @Convert(converter = EncryptedStringConverter.class)
    private String accountNumber;
    
    @Column(nullable = false)
    private String passwordHash; // BCrypt hashed
    
    // ... other fields
}
```

### 3. **API 安全標頭**
```yaml
# Cloud Run 服務配置
apiVersion: serving.knative.dev/v1
kind: Service
metadata:
  annotations:
    run.googleapis.com/ingress: all
spec:
  template:
    metadata:
      annotations:
        run.googleapis.com/network-name: default
        run.googleapis.com/vpc-access-connector: projects/PROJECT_ID/locations/REGION/connectors/CONNECTOR_NAME
```

### 4. **環境變數安全管理**
```bash
# Google Secret Manager 整合
gcloud secrets create db-password --data-file=password.txt

# Cloud Run 服務中使用
gcloud run deploy banking-api \
  --set-env-vars="DB_USER=banking_user" \
  --set-secrets="DB_PASSWORD=db-password:latest"
```

---

## 📋 實施步驟

### 第一階段：準備工作 (Week 1-2)

#### **環境設置**
1. **GCP 專案設置**
   ```bash
   # 建立 GCP 專案
   gcloud projects create banking-app-cloud --name="Banking App Cloud"
   
   # 啟用必要 API
   gcloud services enable run.googleapis.com
   gcloud services enable sql-component.googleapis.com
   gcloud services enable cloudbuild.googleapis.com
   gcloud services enable secretmanager.googleapis.com
   ```

2. **開發環境準備**
   ```bash
   # 安裝必要工具
   curl -L https://github.com/GoogleCloudPlatform/cloud-sql-proxy/releases/download/v1.33.14/cloud_sql_proxy.linux.amd64 -o cloud_sql_proxy
   chmod +x cloud_sql_proxy
   
   # Docker 環境設置
   docker --version
   gcloud auth configure-docker
   ```

#### **程式碼重構準備**
1. **建立新分支**
   ```bash
   git checkout -b feature/cloud-native-refactoring
   ```

2. **更新專案結構**
   ```
   banking-app-cloud/
   ├── services/
   │   ├── auth-service/
   │   ├── account-service/
   │   ├── transaction-service/
   │   └── frontend/
   ├── infrastructure/
   │   ├── terraform/
   │   └── kubernetes/
   ├── scripts/
   └── docs/
   ```

### 第二階段：微服務開發 (Week 3-8)

#### **Auth Service 開發**
1. **建立 Spring Boot 專案**
   ```bash
   spring init \
     --dependencies=web,security,data-jpa,postgresql,redis \
     --package-name=com.banking.auth \
     --name=auth-service \
     auth-service
   ```

2. **實現 JWT 認證**
   ```java
   @RestController
   @RequestMapping("/api/auth")
   public class AuthController {
       
       @PostMapping("/login")
       public ResponseEntity<AuthResponse> login(@RequestBody LoginRequest request) {
           // 實現登入邏輯
       }
       
       @PostMapping("/refresh")
       public ResponseEntity<AuthResponse> refresh(@RequestBody RefreshRequest request) {
           // 實現 token 刷新
       }
   }
   ```

#### **Account Service 開發**
1. **資料模型轉換**
   ```java
   @Entity
   @Table(name = "accounts")
   public class Account {
       @Id
       @GeneratedValue(strategy = GenerationType.IDENTITY)
       private Long accountId;
       
       @Column(unique = true, nullable = false)
       private String accountNumber;
       
       @Column(nullable = false)
       private String customerName;
       
       @Column(unique = true, nullable = false)
       private String email;
       
       @Column(precision = 10, scale = 2)
       private BigDecimal balance;
       
       // ... getters, setters, constructors
   }
   ```

2. **REST API 實現**
   ```java
   @RestController
   @RequestMapping("/api/accounts")
   @Validated
   public class AccountController {
       
       @GetMapping("/{id}")
       public ResponseEntity<AccountDto> getAccount(@PathVariable Long id) {
           // 實現帳戶查詢邏輯
       }
       
       @PutMapping("/{id}")
       public ResponseEntity<AccountDto> updateAccount(
           @PathVariable Long id, 
           @Valid @RequestBody UpdateAccountRequest request) {
           // 實現帳戶更新邏輯
       }
   }
   ```

#### **Transaction Service 開發**
1. **交易處理邏輯**
   ```java
   @Service
   @Transactional
   public class TransactionService {
       
       public Transaction processTransaction(TransactionRequest request) {
           // 驗證帳戶
           Account account = accountService.findById(request.getAccountId());
           
           // 處理交易
           switch (request.getType()) {
               case DEPOSIT:
                   return processDeposit(account, request);
               case WITHDRAWAL:
                   return processWithdrawal(account, request);
               case TRANSFER:
                   return processTransfer(account, request);
               default:
                   throw new IllegalArgumentException("Unsupported transaction type");
           }
       }
   }
   ```

### 第三階段：前端開發 (Week 6-10)

#### **React 應用開發**
1. **專案初始化**
   ```bash
   npx create-react-app frontend --template typescript
   cd frontend
   npm install @mui/material @emotion/react @emotion/styled
   npm install axios react-router-dom @tanstack/react-query
   ```

2. **API 整合**
   ```typescript
   // api/client.ts
   import axios from 'axios';
   
   const apiClient = axios.create({
     baseURL: process.env.REACT_APP_API_BASE_URL,
     timeout: 10000,
   });
   
   apiClient.interceptors.request.use((config) => {
     const token = localStorage.getItem('authToken');
     if (token) {
       config.headers.Authorization = `Bearer ${token}`;
     }
     return config;
   });
   
   export default apiClient;
   ```

3. **主要元件開發**
   ```typescript
   // components/Dashboard.tsx
   export const Dashboard: React.FC = () => {
     const { data: account, isLoading } = useQuery(
       ['account'], 
       () => accountApi.getCurrentAccount()
     );
     
     if (isLoading) return <CircularProgress />;
     
     return (
       <Container>
         <Typography variant="h4">帳戶儀表板</Typography>
         <AccountSummary account={account} />
         <TransactionHistory accountId={account.id} />
       </Container>
     );
   };
   ```

### 第四階段：容器化與部署 (Week 11-12)

#### **建立 Docker 映像**
1. **各服務 Dockerfile**
   ```bash
   # 建構所有服務映像
   ./scripts/build-images.sh
   ```

2. **推送至 Container Registry**
   ```bash
   # 推送至 GCR
   docker push gcr.io/PROJECT_ID/auth-service:latest
   docker push gcr.io/PROJECT_ID/account-service:latest
   docker push gcr.io/PROJECT_ID/transaction-service:latest
   docker push gcr.io/PROJECT_ID/frontend:latest
   ```

#### **Cloud Run 部署**
1. **部署腳本**
   ```bash
   #!/bin/bash
   # deploy.sh
   
   PROJECT_ID="your-project-id"
   REGION="asia-northeast1"
   
   # Deploy Auth Service
   gcloud run deploy auth-service \
     --image gcr.io/$PROJECT_ID/auth-service:latest \
     --platform managed \
     --region $REGION \
     --allow-unauthenticated=false \
     --memory 512Mi \
     --cpu 1 \
     --set-env-vars="SPRING_PROFILES_ACTIVE=cloud"
   
   # Deploy Account Service
   gcloud run deploy account-service \
     --image gcr.io/$PROJECT_ID/account-service:latest \
     --platform managed \
     --region $REGION \
     --allow-unauthenticated=false \
     --memory 512Mi \
     --cpu 1 \
     --set-env-vars="SPRING_PROFILES_ACTIVE=cloud"
   
   # Deploy Transaction Service
   gcloud run deploy transaction-service \
     --image gcr.io/$PROJECT_ID/transaction-service:latest \
     --platform managed \
     --region $REGION \
     --allow-unauthenticated=false \
     --memory 1Gi \
     --cpu 2 \
     --set-env-vars="SPRING_PROFILES_ACTIVE=cloud"
   
   # Deploy Frontend
   gcloud run deploy frontend \
     --image gcr.io/$PROJECT_ID/frontend:latest \
     --platform managed \
     --region $REGION \
     --allow-unauthenticated \
     --memory 256Mi \
     --cpu 1
   ```

### 第五階段：測試與優化 (Week 13-14)

#### **整合測試**
1. **API 測試**
   ```bash
   # 使用 Postman 或 curl 測試
   curl -X POST https://auth-service-xyz.a.run.app/api/auth/login \
     -H "Content-Type: application/json" \
     -d '{"email":"test@example.com","password":"password123"}'
   ```

2. **負載測試**
   ```bash
   # 使用 Apache Bench
   ab -n 1000 -c 10 -H "Authorization: Bearer YOUR_JWT_TOKEN" \
     https://account-service-xyz.a.run.app/api/accounts/1
   ```

#### **效能優化**
1. **資料庫調優**
   ```sql
   -- 建立適當索引
   CREATE INDEX CONCURRENTLY idx_transactions_account_date 
   ON transactions (account_id, transaction_date DESC);
   
   -- 分析查詢效能
   EXPLAIN ANALYZE SELECT * FROM transactions 
   WHERE account_id = 1 ORDER BY transaction_date DESC LIMIT 10;
   ```

2. **快取策略**
   ```java
   @Service
   public class AccountService {
       
       @Cacheable(value = "accounts", key = "#id")
       public Account findById(Long id) {
           return accountRepository.findById(id).orElseThrow();
       }
       
       @CacheEvict(value = "accounts", key = "#account.id")
       public Account save(Account account) {
           return accountRepository.save(account);
       }
   }
   ```

---

## 💰 成本估算

### Cloud Run 服務成本 (每月)
- **Auth Service**: ~$10-20 (低流量)
- **Account Service**: ~$20-40 (中等流量)
- **Transaction Service**: ~$30-60 (高流量)
- **Frontend**: ~$5-15 (靜態內容)

### Cloud SQL 成本 (每月)
- **db-f1-micro**: ~$7-15
- **自動備份**: ~$0.08/GB
- **網路流量**: ~$0.12/GB

### 其他成本
- **Container Registry**: ~$0.10/GB
- **Secret Manager**: ~$0.06/10K operations
- **Cloud Build**: 免費額度內

**總計預估**: $70-150/月 (低到中等流量)

---

## 🎯 成功指標

### 技術指標
- **可用性**: >99.5%
- **回應時間**: <500ms (P95)
- **錯誤率**: <1%
- **自動擴展**: 0-10 個實例

### 業務指標
- **部署頻率**: 每日部署能力
- **修復時間**: <30分鐘
- **變更失敗率**: <5%
- **開發效率**: 提升 50%

---

## 📚 後續規劃

### 短期 (3-6 個月)
- 實施 API Gateway (Cloud Endpoints)
- 新增監控儀表板 (Cloud Monitoring)
- 實施自動化測試套件
- 效能調優與成本優化

### 中期 (6-12 個月)
- 實施 Event-Driven Architecture
- 新增更多業務功能
- 實施 Multi-region 部署
- 進階安全性設定

### 長期 (12+ 個月)
- 考慮 Kubernetes (GKE) 遷移
- 實施 Service Mesh (Istio)
- 機器學習整合 (詐欺檢測)
- 多雲架構探索

---

## 🔗 相關資源

### 官方文件
- [Cloud Run 文件](https://cloud.google.com/run/docs)
- [Cloud SQL 文件](https://cloud.google.com/sql/docs)
- [Spring Boot on GCP](https://cloud.google.com/java/docs/spring)

### 最佳實踐指南
- [12-Factor App](https://12factor.net/)
- [Cloud Native Security](https://cloud.google.com/security/best-practices)
- [Microservices Patterns](https://microservices.io/patterns/)

### 範例專案
- [Spring Boot Samples](https://github.com/spring-projects/spring-boot/tree/main/spring-boot-samples)
- [Google Cloud Samples](https://github.com/GoogleCloudPlatform/java-docs-samples)

---

**📝 文件版本**: v1.0  
**📅 最後更新**: 2025-07-01  
**👤 維護者**: Cloud Architecture Team
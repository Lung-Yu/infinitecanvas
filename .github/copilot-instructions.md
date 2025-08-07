## User Roles

- **Admin**: Full access to all features, including user management and system settings.
- **User**: Limited access to features, primarily focused on their own data and reports.
- **Guest**: Read-only access to public reports and data.

## Execution Guidelines

When encountering a runtime error log, analyze the root cause and develop a strategy before making any modifications.

## 專案架構與技術規範
Domain-Driven Design (DDD) 專案結構，使用 React 前端和 Java Spring 後端：

```
├── frontend/
│   └── CodeTiger/
│       ├── src/
│       │   ├── components/
│       │   ├── pages/
│       │   ├── hooks/
│       │   ├── services/
│       │   ├── stores/
│       │   ├── types/
│       │   └── utils/
│       ├── public/
│       └── package.json
│
├── backend/
│   └── source-analysis/
│       ├── src/main/java/com/tygrus/source/analysis/
│       │   ├── SourceAnalysisApplication.java
│       │   ├── application/
│       │   │   ├── commands/                # 命令處理 (CQRS 寫入操作)
│       │   │   ├── events/                  # 事件處理
│       │   │   ├── queries/                 # 查詢處理 (CQRS 讀取操作)
│       │   │   └── services/
│       │   │       └── use_cases/           # Use Case 業務邏輯
│       │   │           ├── interfaces/      # Use Case 介面定義
│       │   │           └── implementations/ # Use Case 具體實現
│       │   ├── domain/
│       │   │   ├── aggregates/              # 聚合根
│       │   │   ├── entities/                # 領域實體
│       │   │   ├── enums/                   # 領域列舉
│       │   │   ├── exceptions/              # 領域特定異常
│       │   │   ├── repositories/            # Repository 介面
│       │   │   ├── services/                # 領域服務
│       │   │   ├── util/                    # 領域工具類
│       │   │   └── value_objects/           # 值物件
│       │   ├── infrastructure/
│       │   │   ├── adapters/                # 外部適配器
│       │   │   ├── configurations/         # 基礎設施配置
│       │   │   ├── data/                    # 資料存取實作
│       │   │   ├── repositories/           # Repository JPA 實作
│       │   │   ├── security/                # 安全相關配置
│       │   │   └── services/                # 技術服務實作
│       │   └── interfaces/
│       │       ├── controllers/             # REST API 控制器
│       │       ├── dtos/                    # 資料傳輸物件
│       │       │   ├── request/             # 請求 DTO
│       │       │   ├── response/            # 回應 DTO
│       │       │   └── shared/              # 共用 DTO
│       │       ├── mappers/                 # 物件映射器
│       │       └── validators/              # 請求驗證器
│       ├── src/test/java/com/tygrus/source/analysis/
│       │   ├── SourceAnalysisApplicationTests.java
│       │   └── unit/                        # 單元測試
│       │       ├── application/             # Application Layer 測試
│       │       ├── domain/                  # Domain Layer 測試
│       │       ├── infrastructure/         # Infrastructure Layer 測試
│       │       └── interfaces/              # Interface Layer 測試
│       └── pom.xml
│
├── docs/                                    # 專案文檔
│   ├── refactory/                          # 重構追蹤文檔
│   └── *.md                                # 各種技術文檔
├── scripts/                                # 自動化腳本
│   ├── *.sh                               # 各種部署和開發腳本
└── README.md
```

## 技術規範

### 前端技術棧 (Frontend Stack)
- **核心框架**: 
  - React 18.2+
  - TypeScript 5.0+
  - Vite 4.0+ (建構工具)

- **UI 組件與設計系統**:
  - Material-UI (MUI) v5
    - @mui/material - 核心組件
    - @mui/icons-material - 圖示
    - @mui/x-data-grid - 表格組件
    - @mui/x-date-pickers - 日期選擇器
  - 配色方案:
    - Primary: #1976d2 (MUI Blue)
    - Secondary: #9c27b0 (MUI Purple)
    - Error: #d32f2f
    - Warning: #ed6c02
    - Info: #0288d1
    - Success: #2e7d32

- **狀態管理與數據處理**:
  - Redux Toolkit - 全局狀態管理
  - React Query - 服務端狀態管理
  - React Hook Form - 表單處理
  - Yup - 表單驗證

- **工具庫**:
  - Axios - HTTP 請求
  - Recharts - 數據可視化
  - Day.js - 日期處理
  - React Router v6 - 路由管理

### 後端技術棧 (Backend Stack)
- **核心框架**:
  - Spring Boot 3.1+
  - Java 17+
  - Maven

- **核心依賴**:
  - Spring Data JPA
  - Spring Security
  - Spring Cloud
  - Spring Cache
  - MapStruct - 物件映射
  - Lombok - 程式碼簡化
  - Hibernate Validator

- **數據庫**:
  - PostgreSQL
  - Redis (緩存)

- **API 文檔**:
  - SpringDoc OpenAPI (Swagger)

- **監控與日誌**:
  - Spring Actuator
  - Logback
  - Micrometer

- **測試框架**:
  - JUnit 5
  - Mockito
  - TestContainers


### 開發規範
- 使用 ESLint 和 Prettier 進行程式碼格式化
- 遵循 Material Design 設計規範
- 實現響應式設計 (Desktop First)
- 支援深色模式
- 國際化支援 (i18n)
- 遵循 SOLID 原則和 Clean Architecture
- **Java 檔案結構強制要求**：
  - **一個 `.java` 檔案只能包含一個主要的 class/interface/enum**
  - **禁止在 interface 檔案中定義內部 class**：所有內部類別必須提取為獨立檔案
  - **內部類別提取規範**：
    - DTO 類別 → `interfaces/dtos/request/` 或 `interfaces/dtos/response/`
    - Enum 類別 → `domain/enums/`
    - Exception 類別 → `domain/exceptions/`
    - Value Object 類別 → `domain/value_objects/`
  - **重構驗證要求**：重構後必須透過 `mvn compile` 驗證沒有編譯錯誤
  - **Import 更新責任**：確保所有 import 語句正確更新，檢查沒有 Bean 定義衝突
- **後端開發採用 Code First 策略**：
  - **領域模型優先**：專注於領域模型與業務邏輯設計，資料表結構由實體類自動生成與維護
  - **業務邏輯分割**：Domain Entity 與資料庫 Table 沒有絕對對應關係，依據業務概念進行邏輯分割
  - **避免資料庫驅動設計**：不以資料庫表結構為出發點設計實體，而是以業務需求為導向
  - **JPA 映射靈活性**：利用 JPA 註解 (@OneToMany, @Embedded, @SecondaryTable 等) 實現複雜的實體-表映射關係
- **開發優先採用 @Annotation 解決方案**：
  - 優先利用 Spring、JPA、Validation、AOP 等框架的註解（@Component、@Service、@Repository、@Transactional、@Entity、@Value、@Validated、@Cacheable 等）來簡化設定、切面、驗證、快取、權限、日誌等橫切關注點，減少樣板程式碼，提升可維護性與一致性。
  - 自訂義註解時，需有明確的使用情境與 JavaDoc，並於必要時搭配 AOP 或 Validator 實現。
  - 減少重複程式碼與手動註冊，優先考慮註解驅動的自動化與宣告式設計。

## DDD 架構層級開發準則

### Application Layer 設計準則 (Use Case 模式實現)

#### 1. Use Case 介面與實作設計
**強制要求**: 所有業務邏輯必須通過 Use Case 介面實現，禁止直接呼叫技術服務

**目錄結構與命名規範**：
```
application/
├── services/
│   └── use_cases/
│       ├── interfaces/          # Use Case 介面定義
│       │   ├── [Domain]ManagementUseCase.java
│       │   └── [Domain]QueryUseCase.java
│       └── implementations/     # Use Case 具體實現
│           ├── [Domain]ManagementUseCaseImpl.java
│           └── [Domain]QueryUseCaseImpl.java
```

**實作範例**：
```java
// Use Case 介面：專注業務操作定義
@Component
public interface DetectionRuleManagementUseCase {
    Page<DetectionRule> searchDetectionRules(DetectionRuleSearchRequest request, Pageable pageable);
    DetectionRule getDetectionRuleById(Long id);
    Map<String, Long> getDetectionRuleStatistics();
}

// Use Case 實現：統籌領域邏輯，返回領域實體
@Service
public class DetectionRuleManagementUseCaseImpl implements DetectionRuleManagementUseCase {
    
    private static final Logger log = LoggerFactory.getLogger(DetectionRuleManagementUseCaseImpl.class);
    private final DetectionRuleRepository detectionRuleRepository;
    
    @Override
    @Transactional(readOnly = true)
    public Page<DetectionRule> searchDetectionRules(DetectionRuleSearchRequest request, Pageable pageable) {
        try {
            return detectionRuleRepository.findBySearchCriteria(request, pageable);
        } catch (Exception e) {
            throw new DetectionRuleOperationException("搜尋檢測規則失敗", e);
        }
    }
}
```

#### 2. Controller 層職責定義
**強制要求**: Controller 只能呼叫 Use Case 介面，不得直接呼叫技術服務或 Repository

**Controller 職責**：
1. HTTP 請求參數驗證和轉換
2. 呼叫適當的 Use Case 介面
3. 將領域實體轉換為 DTO 回應格式
4. 異常處理和 HTTP 狀態碼設定

**實作範例**：
```java
@RestController
@RequestMapping("/api/detection-rules")
@RequiredArgsConstructor
@Slf4j
public class DetectionRuleController {

    private final DetectionRuleManagementUseCase detectionRuleManagementUseCase;

    @GetMapping
    public ResponseEntity<DetectionRuleListResponse> searchDetectionRules(
            @ModelAttribute DetectionRuleSearchRequest searchRequest,
            @RequestParam(defaultValue = "0") int page,
            @RequestParam(defaultValue = "20") int size) {
        
        try {
            // 1. 建立分頁請求
            final Pageable pageable = PageRequest.of(page, size, Sort.by("id").descending());
            
            // 2. 呼叫 Use Case (返回 Domain Entity)
            final Page<DetectionRule> rulePage = detectionRuleManagementUseCase
                .searchDetectionRules(searchRequest, pageable);
            
            // 3. 轉換為 DTO 回應
            final List<DetectionRuleDto> ruleDtos = rulePage.getContent().stream()
                .map(DetectionRuleMapper::toDto)
                .collect(Collectors.toList());
            
            return ResponseEntity.ok(new DetectionRuleListResponse(ruleDtos, rulePage));
            
        } catch (DetectionRuleOperationException e) {
            log.error("搜尋檢測規則失敗", e);
            return ResponseEntity.badRequest().build();
        }
    }
}
```

#### 3. Controller 職責與 DTO 轉換規範
**Controller 職責範圍**：
1. **請求參數驗證**: 使用 `@Valid` 和 `@Validated` 註解
2. **DTO 到領域物件轉換**: 只在必要時進行，多數情況直接傳遞 Request DTO
3. **Use Case 呼叫**: 業務邏輯統一透過 Use Case 介面執行
4. **領域物件到 Response DTO 轉換**: 使用 Mapper 轉換為回應格式
5. **HTTP 狀態碼設定**: 根據業務結果設定適當的 HTTP 狀態

**DTO 轉換最佳實踐**：
```java
@RestController
@RequestMapping("/api/detection-rules")
public class DetectionRuleController {

    @PostMapping
    public ResponseEntity<DetectionRuleDto> createDetectionRule(
            @Valid @RequestBody DetectionRuleCreateRequest request) {
        try {
            // 1. 直接傳遞 Request DTO 給 Use Case
            DetectionRule rule = useCase.createDetectionRule(request);
            
            // 2. 領域實體轉換為 Response DTO
            DetectionRuleDto response = DetectionRuleMapper.toDto(rule);
            
            return ResponseEntity.status(HttpStatus.CREATED).body(response);
        } catch (DetectionRuleValidationException e) {
            return ResponseEntity.badRequest().build();
        }
    }
    
    @GetMapping
    public ResponseEntity<DetectionRuleListResponse> searchDetectionRules(
            @ModelAttribute @Valid DetectionRuleSearchRequest request,
            @RequestParam(defaultValue = "0") int page,
            @RequestParam(defaultValue = "20") int size) {
        
        Pageable pageable = PageRequest.of(page, size, Sort.by("id").descending());
        Page<DetectionRule> result = useCase.searchDetectionRules(request, pageable);
        
        // 使用專用的 Response DTO 封裝分頁結果
        DetectionRuleListResponse response = new DetectionRuleListResponse(result);
        return ResponseEntity.ok(response);
    }
}
```

#### 4. Mapper 設計與實作規範
```java
// 使用 MapStruct 進行物件映射
@Mapper(componentModel = "spring")
public interface DetectionRuleMapper {
    
    DetectionRuleDto toDto(DetectionRule entity);
    
    List<DetectionRuleDto> toDtoList(List<DetectionRule> entities);
    
    // 複雜映射邏輯可使用 @Mapping 註解
    @Mapping(target = "categoryName", source = "category.name")
    @Mapping(target = "createdBy", source = "audit.createdBy")
    DetectionRuleSummaryDto toSummaryDto(DetectionRule entity);
}
```

### Domain Layer 設計準則

#### 1. 領域實體設計原則
**核心概念**: Domain Entity 與資料庫 Table 沒有絕對的對應關係，應基於業務邏輯進行分割

**實體設計策略**：
```java
// ✅ 正確：基於業務概念的實體設計
@Entity
public class DetectionRule {
    // 業務核心屬性
    private String name;
    private String description;
    private RuleCategory category;
    private RuleStatus status;
    private RuleComplexity complexity;
    
    // 業務行為方法
    public void activate() { /* 業務邏輯 */ }
    public boolean isApplicableFor(CodeLanguage language) { /* 業務邏輯 */ }
    public RuleExecutionResult execute(SourceCode code) { /* 業務邏輯 */ }
}

// ❌ 錯誤：直接對應資料庫表結構
@Entity
public class DetectionRuleTable {
    private String rule_name;
    private String rule_desc;
    private Integer category_id;
    private Integer status_code;
    // 缺乏業務行為，僅為資料容器
}
```

**邏輯分割實例**：
- **單一資料庫表可拆分為多個領域實體**：
  ```java
  // user_profiles 表拆分為多個業務實體
  public class UserAccount { /* 帳戶相關邏輯 */ }
  public class UserPreferences { /* 偏好設定邏輯 */ }
  public class UserSecurityProfile { /* 安全相關邏輯 */ }
  ```

- **多個資料庫表可合併為單一領域實體**：
  ```java
  // orders + order_items + order_payments 表合併
  public class Order {
      private List<OrderItem> items;
      private PaymentInfo payment;
      
      public Money calculateTotal() { /* 跨表業務邏輯 */ }
      public void processPayment() { /* 跨表業務邏輯 */ }
  }
  ```

#### 2. 聚合根設計
- 每個聚合都必須有清楚的業務邊界
- 聚合根負責維護業務不變量
- 跨聚合操作必須透過領域服務協調

#### 3. 值物件設計
- 不可變性：一旦創建就不能修改
- 值相等性：根據屬性值判斷相等性
- 自我驗證：在建構時進行業務規則驗證

#### 4. 領域特定異常設計
   - 所有業務異常都必須定義在 `domain/exceptions` 目錄
   - 異常名稱必須具體描述業務場景：`[Domain]NotFoundException`, `[Domain]OperationException`
   - 提供業務上下文資訊，如實體 ID、操作類型等
   - 支援異常鏈，保留根本原因的技術細節

### Interfaces Layer 設計準則

#### 1. 檔案結構與內部類別禁止規範
**強制要求**: 嚴格遵循一個檔案一個類別的原則，禁止內部類別定義

**錯誤示例**：
```java
// ❌ 錯誤：在 interface 檔案中包含內部類別
public interface VulnerabilitySuppressionUseCase {
    SuppressionResult suppressVulnerability(Long id);
    
    // 禁止：內部類別定義
    class SuppressionResult { }
    class SuppressionStatistics { }
}
```

**正確示例**：
```java
// ✅ 正確：純粹的介面定義
public interface VulnerabilitySuppressionUseCase {
    SuppressionResult suppressVulnerability(Long id);
    SuppressionStatistics getStatistics();
}
```

**重構策略**：
```
// 重構前：一個檔案包含多個類別
VulnerabilitySuppressionUseCase.java
├── interface VulnerabilitySuppressionUseCase
├── class SuppressionResult
└── class SuppressionStatistics

// 重構後：分離為獨立檔案
use_cases/interfaces/VulnerabilitySuppressionUseCase.java
dtos/response/SuppressionResult.java
dtos/response/SuppressionStatistics.java
```

#### 2. DTO 分類與組織結構
```java
interfaces/dtos/
├── request/                    # 請求 DTO
│   ├── [Domain]CreateRequest.java
│   ├── [Domain]UpdateRequest.java
│   ├── [Domain]SearchRequest.java
│   └── [Domain]FilterRequest.java
├── response/                   # 回應 DTO
│   ├── [Domain]Dto.java        # 單一實體回應
│   ├── [Domain]ListResponse.java  # 列表回應（含分頁資訊）
│   ├── [Domain]SummaryDto.java # 摘要資訊回應
│   └── [Domain]StatisticsResponse.java  # 統計資訊回應
└── shared/                     # 共用 DTO
    ├── PageableResponse.java   # 分頁回應格式
    ├── ErrorResponse.java      # 錯誤回應格式
    └── ApiResponse.java        # 統一 API 回應包裝
```

**實作範例**：
```java
// Request DTO - 請求參數封裝
public class DetectionRuleSearchRequest {
    private String name;
    private String category;
    private String status;
    private LocalDateTime createdAfter;
    private LocalDateTime createdBefore;
    
    // getters, setters, validation annotations
}

// Response DTO - 回應資料封裝
public class DetectionRuleListResponse {
    private List<DetectionRuleDto> rules;
    private PageMetadata pageInfo;
    private Long totalElements;
    private Integer totalPages;
    
    public DetectionRuleListResponse(Page<DetectionRule> page) {
        this.rules = page.getContent().stream()
            .map(DetectionRuleMapper::toDto)
            .collect(Collectors.toList());
        this.pageInfo = new PageMetadata(page);
        this.totalElements = page.getTotalElements();
        this.totalPages = page.getTotalPages();
    }
}
```

#### 3. Controller 職責與 DTO 轉換規範
**Controller 職責範圍**：
1. **請求參數驗證**: 使用 `@Valid` 和 `@Validated` 註解
2. **DTO 到領域物件轉換**: 只在必要時進行，多數情況直接傳遞 Request DTO
3. **Use Case 呼叫**: 業務邏輯統一透過 Use Case 介面執行
4. **領域物件到 Response DTO 轉換**: 使用 Mapper 轉換為回應格式
5. **HTTP 狀態碼設定**: 根據業務結果設定適當的 HTTP 狀態

**DTO 轉換最佳實踐**：
```java
@RestController
@RequestMapping("/api/detection-rules")
public class DetectionRuleController {

    @PostMapping
    public ResponseEntity<DetectionRuleDto> createDetectionRule(
            @Valid @RequestBody DetectionRuleCreateRequest request) {
        try {
            // 1. 直接傳遞 Request DTO 給 Use Case
            DetectionRule rule = useCase.createDetectionRule(request);
            
            // 2. 領域實體轉換為 Response DTO
            DetectionRuleDto response = DetectionRuleMapper.toDto(rule);
            
            return ResponseEntity.status(HttpStatus.CREATED).body(response);
        } catch (DetectionRuleValidationException e) {
            return ResponseEntity.badRequest().build();
        }
    }
    
    @GetMapping
    public ResponseEntity<DetectionRuleListResponse> searchDetectionRules(
            @ModelAttribute @Valid DetectionRuleSearchRequest request,
            @RequestParam(defaultValue = "0") int page,
            @RequestParam(defaultValue = "20") int size) {
        
        Pageable pageable = PageRequest.of(page, size, Sort.by("id").descending());
        Page<DetectionRule> result = useCase.searchDetectionRules(request, pageable);
        
        // 使用專用的 Response DTO 封裝分頁結果
        DetectionRuleListResponse response = new DetectionRuleListResponse(result);
        return ResponseEntity.ok(response);
    }
}
```

#### 4. Mapper 設計與實作規範
```java
// 使用 MapStruct 進行物件映射
@Mapper(componentModel = "spring")
public interface DetectionRuleMapper {
    
    DetectionRuleDto toDto(DetectionRule entity);
    
    List<DetectionRuleDto> toDtoList(List<DetectionRule> entities);
    
    // 複雜映射邏輯可使用 @Mapping 註解
    @Mapping(target = "categoryName", source = "category.name")
    @Mapping(target = "createdBy", source = "audit.createdBy")
    DetectionRuleSummaryDto toSummaryDto(DetectionRule entity);
}
```

### Infrastructure Layer 設計準則
1. **Repository 實作**：
   - JPA 實作放置於 `infrastructure/repositories/jpa`
   - 快取實作放置於 `infrastructure/repositories/cache`
   - 實作必須遵循領域層定義的 Repository 介面

2. **技術服務統一管理**：
   - 所有技術服務 (AuditService, FileService 等) 都放置於 Infrastructure Layer
   - 避免在 Application Layer 重複定義技術服務，防止 Bean 衝突
   - 外部 API 整合放置於 `infrastructure/services/external`
   - 檔案處理服務放置於 `infrastructure/services/file`
   - 通知服務放置於 `infrastructure/services/notification`

### 使用案例測試要求
- 每個 Use Case 都必須有對應的使用案例測試
- 測試類別命名格式：`{UseCaseName}UseCaseTest`
- 專注於驗證業務邏輯而非技術實作細節
- 測試應涵蓋正常流程和異常處理流程
- 所有查詢測試都必須驗證分頁機制的正確性
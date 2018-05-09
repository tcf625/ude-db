# Introduction

UDE 的資料庫相關元件，為幾個常見的資料庫API定義一組統一使用介面，包括spring jdbc template / hibernate / jdbc。

並提供一些稽核資訊的接口及未釋放資源的輔助管理。

另外，本模組中定義的一些輔助結構，如QueryExecutor有助於提升專案產品的維護性及可測試性。或者可以讓專案在初期使用HIBERNATE進行快速開發，到中後期才低成本的轉換為JDBC底層實作以提升效能。

test


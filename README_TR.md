# Qualyx PhoneXR v0.6 — gerçek Vulkan swapchain köprüsü

Bu sürüm, PhoneXR'ın Android/ARM64 native host ve OpenXR/Vulkan sınırını daha güvenli hale getirir.

## v0.6'da değişenler

- Native APK sürümü `0.5.0`.
- GitHub Actions ARM64 debug APK üretir.
- OpenXR extension enumeration artık `XR_KHR_vulkan_enable2` ve `XR_KHR_vulkan_enable` döndürüyor.
- Frame lifecycle ve stereo view temel state ile takip ediliyor.
- `xrGetVulkanGraphicsDevice2KHR()` gerçek Vulkan physical device seçiyor.
- Uygulamanın verdiği `VkInstance/VkPhysicalDevice/VkDevice` graphics binding session state içine alınır.
- `xrEnumerateSwapchainFormats()` gerçek fiziksel cihaz format desteğine göre cevap verir.
- Runtime gerçek `VkImage` oluşturur, `VkDeviceMemory` ayırır ve image belleğini bağlar.
- `xrEnumerateSwapchainImages()`, acquire/wait/release lifecycle artık gerçek image handle döndürür.
- `xrPollEvent()` ilk session için `XR_SESSION_STATE_READY` lifecycle event üretir.
- Vulkan/OpenXR structure type değerleri Khronos tanımlarına göre düzeltildi.

## Sonraki teknik hedef

Bir sonraki aşama Android `ANativeWindow` üzerinde gerçek Vulkan presentation/compositor yolunu kurmak: runtime swapchain image'larını hedef surface'e kopyalayacak/present edecek, ardından head pose/controller input ve composition layer desteği genişletilecek.

Bu aşama tamamlanmadan Half-Life: Alyx'in oynanabilir olduğu iddia edilmemelidir.

Qualyx/Alyx'ın telifli APK'sını repository'ye koymayın. Kendi yasal kopyanız üzerinde patch workflow'u kullanın.

## v0.8 — Alyx host uyumluluğu + Adreno 830 hazırlığı

`lib.zip` içindeki gerçek `libhlvr_host.so` incelenerek action/path/controller/haptic ve Android performance API'leri eklendi. Bu çağrılar şu anda güvenli compatibility/no-op katmanı kullanır; henüz gerçek cihaz IMU/controller backend'i değildir.

Adreno tarafında runtime, fiziksel cihazı ve desteklenen Vulkan uzantılarını açılışta raporlar. Özellikle `VK_QCOM_tile_memory_heap` ve mesh shading varlığı algılanır. Tile-memory kullanımı otomatik zorlanmıyor; çünkü XR swapchain kaynaklarının yaşam süresi ve compositor senkronizasyonu doğrulanmadan bu belleğe bağlamak güvenli değil.

Qualcomm'un güncel dokümantasyonuna göre Snapdragon 8 Elite Gen 5 sınıfı Adreno GPU'larda Vulkan 1.3 ve Tile Memory Heap gibi özellikler mevcut; tile memory yüksek trafik alan, kısa ömürlü kaynaklarda bant genişliğini azaltmak için hedefleniyor. Runtime bu özelliği tespit edip sonraki compositor aşamasında seçici kullanıma hazırlıyor.

## v0.8 — gerçek payload analizi

Sağlanan `lib.zip` içindeki `librendersystemvulkan.so` ve `libvulkan_freedreno_hlvr.so` incelendi. Bunların Linux/SteamRT ARM64 olduğu doğrulandı; Android'de doğrudan yüklenmeleri hedeflenmiyor. PhoneXR bunları Android'e taşımaya çalışmak yerine Vulkan API sınırını yeniden kullanıyor.

Android cihazdaki GPU/driver yetenekleri runtime açılışında sorgulanıyor. Adreno/Qualcomm uzantıları mevcutsa yalnızca raporlanıyor; güvenli bir feature-enable aşaması sonraki compositor çalışmasında yapılacak.

# PhoneXR v0.9

## Android Vulkan compositor bring-up

Adds an Android `ANativeWindow` compositor path. When the host Vulkan instance/device exposes `VK_KHR_swapchain` and the queue family can present to an Android surface, PhoneXR creates an Android `VkSurfaceKHR` + `VkSwapchainKHR` and presents the first runtime swapchain image through a GPU blit.

This is intentionally a bring-up path, not the final stereo compositor:
- uses FIFO present mode for portability;
- waits on the queue for conservative synchronization;
- uses a single source image as the first compatibility path;
- full OpenXR projection-layer/eye routing, distortion, and asynchronous reprojection are still next.

## Adreno 830

No Qualcomm extension is force-enabled. Device capabilities are queried and the compositor stays on standard Vulkan WSI until the exact device/driver reports support. This avoids making unsupported tile-memory assumptions.

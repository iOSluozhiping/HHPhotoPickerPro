# 仿微信相册选择器接入文档

# 1,使用pod方式集成：
    pod "HHPhotoPickerPro_github"
    
# 2,导入头文件
    import HHPhotoPicker
    import Photos
    注：如果不实现HHPhotoPickerManagerDelegate的selectImageRequestError协议可以不用import Photos
    
# 3,调用示例
    self.pickerManager = HHPhotoPickerManager(showVC: self.showVC);
    self.pickerManager?.photoUIConfigModel = self.uiConfig
    self.pickerManager?.photoConfigModel = self.config
    self.pickerManager?.viewDelegate = self.showVC as? HHPhotoPickerManagerDelegate;
    self.pickerManager?.showImagePicker()
    注：(1)showVC和viewDelegate必传 (2)photoUIConfigModel和photoConfigModel可配置参数，具体参数定义请看下面说明。
    
# 4,其它说明
    HHPhotoPickerManager自定义参数：
    
    public class HHPhotoConfigModel: NSObject {
        
        // 最大预览张数
        public var maxPreviewCount = 20;
        
        // 最大选择张数
        private var pri_maxSelectCount = 9
        
        public var maxSelectCount: Int {
            get {
                return pri_maxSelectCount
            }
            set {
                pri_maxSelectCount = max(1, newValue)
            }
        }
        
        // 视频最小选择个数
        private var pri_minVideoSelectCount = 0

        public var minVideoSelectCount: Int {
            get {
                return min(maxSelectCount, max(pri_minVideoSelectCount, 0))
            }
            set {
                pri_minVideoSelectCount = newValue
            }
        }
        
        // 视频最大选择个数
        private var pri_maxVideoSelectCount = 0
        
        public var maxVideoSelectCount: Int {
            get {
                if pri_maxVideoSelectCount <= 0 {
                    return maxSelectCount
                } else {
                    return max(minVideoSelectCount, min(pri_maxVideoSelectCount, maxSelectCount))
                }
            }
            set {
                pri_maxVideoSelectCount = newValue
            }
        }
        
        // 视频最小选择时长
        public var minSelectVideoDuration: Second = 0
        
        // 视频最大选择时长
        public var maxSelectVideoDuration: Second = 120
        
        // cell圆角
        public var cellCornerRadio: CGFloat = 0
        
        // 框架语言
        public var languageType: ZLLanguageType = .system {
            didSet {
                ZLCustomLanguageDeploy.language = self.languageType
                Bundle.resetLanguage()
            }
        }
        
        // 每行显示照片个数(每列个数)
        private var pri_columnCount: Int = 4
        
        public var columnCount: Int {
            get {
                return pri_columnCount
            }
            set {
                pri_columnCount = min(6, max(newValue, 2))
            }
        }
        
        // 排序方式 默认升序
        public var sortAscending = true
        
        // 允许选择图片
        public var allowSelectImage = true
        
        // 允许相册内部拍照
        public var allowTakePhotoInLibrary = false
        
        // 允许选择原图
        public var allowSelectOriginal = false
            
        // 允许选择Gif
        public var allowSelectGif = false
        
        // 允许选择视频
        public var allowSelectVideo = false

        // 允许选择livephoto
        public var allowSelectLivePhoto = false
        
        // 允许编辑图片
        public var allowEditImage = false
        
        // 允许图片视频一起选择
        public var allowMixSelect = true
        
        // 允许进入大图界面
        public var allowPreviewPhotos = true
        
        // 涂鸦（只有允许编辑图片才生效）
        public var editImageWithDraw = false
        
        // 裁剪（只有允许编辑图片才生效）
        public var editImageWithClip = false
        
        // 贴图（只有允许编辑图片才生效）
        public var editImageWithImageSticker = false
        
        // 文本（只有允许编辑图片才生效）
        public var editImageWithTextSticker = false
        
        // 马赛克（只有允许编辑图片才生效）
        public var editImageWithMosaic = false
        
        // 滤镜（只有允许编辑图片才生效）
        public var editImageWithFilter = false
        
        // 色值调整（只有允许编辑图片才生效）
        public var editImageWithAdjust = false
        
        // 亮度（只有允许色值调整才生效）
        public var editImageWitAdjustBrightness = false
        
        // 对比度（只有允许色值调整才生效）
        public var editImageWitAdjustContrast = false
        
        // 饱和度（只有允许色值调整才生效）
        public var editImageWitAdjustSaturation = false
        
        public var shouldAnialysisAsset = true

        // 允许编辑视频
        var pri_allowEditVideo = false
        
        public var allowEditVideo: Bool {
            get {
                return pri_allowEditVideo && shouldAnialysisAsset
            }
            set {
                pri_allowEditVideo = newValue
            }
        }
        
        // 保存编辑的图片
        public var saveNewImageAfterEdit = true
        
        // 允许拖拽选择
        public var allowDragSelect = false
        
        // 允许滑动选择
        public var allowSlideSelect = true

        // 滑动选择时自动滚动
        public var autoScrollWhenSlideSelectIsActive = true

        // 自动滚动最大速度
        public var autoScrollMaxSpeed: CGFloat = 600

        // 拍照cell显示相机俘获画面
        public var showCaptureImageOnTakePhotoBtn = false

        // 显示已选择照片index
        public var showSelectedIndex = true

        // 显示已选择照片遮罩
        public var showSelectedMask = true

        // 显示已选择照片边框
        public var showSelectedBorder = false

        // 显示不可选状态照片遮罩
        public var showInvalidMask = true

        // 使用自定义相机
        public var useCustomCamera = true
        
        // 闪光灯
        public var flashMode: ZLCameraConfiguration.FlashMode = .off
        
    }

    public class HHPhotoUIConfigModel: NSObject {
        
        // 相册样式：样式一(仿微信) 样式二(传统）
        public var style: ZLPhotoBrowserStyle = .embedAlbumList
        
        // 相册小图界面底部按钮可交互状态下背景色
        public var bottomToolViewBtnNormalBgColor = zlRGB(80, 169, 56)
        
        // 预览大图界面底部按钮可交互状态下背景色
        public var bottomToolViewBtnNormalBgColorOfPreviewVC = zlRGB(80, 169, 56)
        
        // 已选照片右上角序号label背景色
        @objc public var indexLabelBgColor = zlRGB(80, 169, 56)

    }
    
    注：此SDK暂时只支持真机

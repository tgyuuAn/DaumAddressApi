<img width="876" alt="image" src="https://github.com/user-attachments/assets/94549940-7f37-4bd9-894b-b4f81c76158f">

[다음 우편번호 무료 API](https://postcode.map.daum.net/guide)

<br><br><br>

안드로이드에서 위 화면을 뛰우기 위해서는 **WebView**를 사용해야 함.

웹뷰를 쓰던 뭘 하던 간에 사전에 HTML 코드를 서버에서 등록하고 클라이언트에서 받아서 써야하는데,

굳이 힘들게 그럴 필요 없이 [GitHub Pages로 HTML 배포하기](https://velog.io/@commi1106/Github-Pages%EB%A1%9C-HTML-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0) 로 손쉽게 사용가능.

<br><br><br>

**WebView**를 사용할 때 url을`https://tgyuuan.github.io/DaumAddressApi`로 설정하고 사용하면 된다.

```kotlin
@AndroidEntryPoint
class PostCodeFragment : DialogFragment() {

    private var _binding: FragmentPostCodeBinding? = null
    private val binding get() = _binding!!

    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View {
        _binding = FragmentPostCodeBinding.inflate(inflater, container, false)
        binding.lifecycleOwner = this.viewLifecycleOwner
        return binding.root
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        setupWebView()
    }

    override fun onDestroyView() {
        super.onDestroyView()
        _binding = null
    }

    @SuppressLint("SetJavaScriptEnabled")
    private fun setupWebView() {
        binding.postcodeWV.apply {
            settings.javaScriptEnabled = true
            settings.domStorageEnabled = true
            settings.useWideViewPort = true
            settings.loadWithOverviewMode = true
            settings.cacheMode = WebSettings.LOAD_NO_CACHE

            webViewClient = WebViewClient()
            webChromeClient = WebChromeClient()

            addJavascriptInterface(AndroidBridge(), "android")
            loadUrl("https://tgyuuan.github.io/DaumAddressApi")
        }
    }

    private inner class AndroidBridge {
        @JavascriptInterface
        fun onPostCodeReceived(roadNameAddress: String, lotNumberAddress: String) {
            
            // 여기서 받아온 데이터를 핸들링 할 코드를 작성

            dismiss()
        }
    }
}
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <FrameLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".PostCodeFragment">

        <WebView
            android:id="@+id/postcode_WV"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:layout_margin="10dp" />
    </FrameLayout>
</layout>
```
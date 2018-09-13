---
title: "Kotlin - anko"
date: 2018-09-13
categories: kotlin
---

Kotlin-anko
-

의미
-
: Anko (Kotlin), A library used in Android applications written in Kotlin. by wikipedia

https://github.com/Kotlin/anko/wiki

> Anko is a Kotlin library which makes Android application development faster and easier. It makes your code clean and easy to read, and lets you forget about rough edges of Android SDK for Java.


정식 1.xx 버전 나오지 않음
 -> 현재 버전 : 0.10.5 (2018.09.13 기준)


Gradle 선언
-
```java
//Anko
    def anko_version='0.10.5'
    // Anko Commons
    implementation "org.jetbrains.anko:anko-commons:$anko_version"
    // Anko Layouts
    implementation "org.jetbrains.anko:anko-sdk25:$anko_version" // sdk15, sdk19, sdk21, sdk23 are also available
    implementation "org.jetbrains.anko:anko-appcompat-v7:$anko_version"
    implementation "org.jetbrains.anko:anko-constraint-layout:$anko_version"
    implementation "org.jetbrains.anko:anko-recyclerview-v7:$anko_version"
```

kotlin code
-
```java
class MediaLiveOnActivity  : BaseActivity() {
   
    private val TAG = MediaLiveOnActivity::class.java.simpleName

    private lateinit var binding : ActivityMediaLiveOnBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        var adapter: ChatListAdapter = ChatListAdapter(this, object : ChatListAdapter.OnItemClickListener {
            override fun onItemClick(item: ChatData?) {
                toast("item click")
            }
        })
        MediaLiveOnActivityUI(adapter).setContentView(this)
    }


class MediaLiveOnActivityUI(val listAdapter: ChatListAdapter)  : AnkoComponent<MediaLiveOnActivity> {
    override fun createView(ui: AnkoContext<MediaLiveOnActivity>)= with(ui) {
        constraintLayout {
            lparams(matchParent, matchParent)
            padding = dip(7)
            backgroundColor = Color.DKGRAY

            var list = recyclerView {
                layoutManager = LinearLayoutManager(context)
                adapter = listAdapter
            }.lparams(width = dip(207), height = matchParent){
//                topPadding = dip(32)
//                bottomPadding = dip(36)
                clipToPadding = false
            }

            linearLayout {
                orientation = LinearLayout.HORIZONTAL
                leftPadding=dip(7)
                verticalGravity= Gravity.CENTER_VERTICAL
                leftPadding=dip(9)
                setBackgroundResource(R.drawable.chat_outline_rectangle)
                textView ("LIVE"){
                    textSize = 12f
                    typeface = Typeface.DEFAULT_BOLD
                    textColor = Color.WHITE
                }

            }.lparams(dip(207), dip(32)){
                horizontalBias=0.0f
                endToEnd=ConstraintLayout.LayoutParams.PARENT_ID
                topToTop= ConstraintLayout.LayoutParams.PARENT_ID
                startToStart= ConstraintLayout.LayoutParams.PARENT_ID
            }

            linearLayout {
                setBackgroundResource(R.drawable.chat_outline_rectangle)
                val input = editText {
                    id = R.id.et_chat
                    gravity=  Gravity.CENTER_VERTICAL
                    textColor = Color.LTGRAY
                    hintTextColor= Color.DKGRAY
                    hint="메세지를 입력하세요."
                    textSize = 10.5f
                    imeOptions=EditorInfo.IME_FLAG_NO_FULLSCREEN
                    leftPadding=dip(9)
                }.lparams(width = dip(207), weight = 1f)

                val sendButton = imageButton {
                    id = R.id.bt_chat_send
                    setImageResource(R.drawable.ic_emoticon)
                    backgroundColor=Color.TRANSPARENT
                    rightPadding=dip(3)
                }.lparams(wrapContent, wrapContent)

                sendButton.setOnClickListener {
                    val data = input.text.toString()
                    val adpater = list?.adapter as RecyclerView.Adapter
                    if(!data.isBlank()) {
                        val data = ChatData("guest", data);
                        listAdapter.addLast(data)
                    }

                }
            }.lparams(dip(207), dip(32)) {
                horizontalBias=0.0f
                endToEnd= ConstraintLayout.LayoutParams.PARENT_ID
                bottomToBottom= ConstraintLayout.LayoutParams.PARENT_ID
                startToStart= ConstraintLayout.LayoutParams.PARENT_ID
            }

        }
    }
}
```

java code
-
java, xml 파일 필요, 상세 내용은 적지 않겠음..




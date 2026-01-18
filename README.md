
Ayanix/
├── codemagic.yaml
├── app/
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/com/ayanix/
│   │   │   │   ├── MainActivity.kt
│   │   │   │   ├── SignalEngine.kt
│   │   │   ├── res/
│   │   │   │   ├── layout/
│   │   │   │   │   ├── activity_main.xml
│   │   │   │   │   ├── activity_signal.xml
│   │   │   │   │   ├── activity_settings.xml
│   │   │   │   ├── values/
│   │   │   │   │   ├── colors.xml
│   │   │   │   │   ├── strings.xml
│   │   │   │   │   ├── styles.xml
│   │   ├── AndroidManifest.xml
├── build.gradle
├── settings.gradle
[1/18, 10:41 PM] Ayan Butt: package com.ayanix

object SignalEngine {

    fun getSignal(emaFast: Double, emaSlow: Double, rsi: Double, candleBullish: Boolean, volatilityOk: Boolean): String {
        if(!volatilityOk) return "NO TRADE"

        if(emaFast > emaSlow && rsi in 25.0..35.0 && candleBullish) return "BUY"
        if(emaFast < emaSlow && rsi in 65.0..75.0 && !candleBullish) return "SELL"

        return "NO TRADE"
    }
}
[1/18, 10:41 PM] Ayan Butt: package com.ayanix

import android.content.Intent
import android.os.Bundle
import android.widget.Button
import android.widget.Spinner
import androidx.appcompat.app.AppCompatActivity

class MainActivity : AppCompatActivity() {

    lateinit var marketSpinner: Spinner
    lateinit var timeSpinner: Spinner
    lateinit var startBtn: Button

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        marketSpinner = findViewById(R.id.marketSpinner)
        timeSpinner = findViewById(R.id.timeSpinner)
        startBtn = findViewById(R.id.startBtn)

        startBtn.setOnClickListener {
            val intent = Intent(this, SignalActivity::class.java)
            intent.putExtra("market", marketSpinner.selectedItem.toString())
            intent.putExtra("time", timeSpinner.selectedItem.toString())
            startActivity(intent)
        }
    }
}
[1/18, 10:42 PM] Ayan Butt: package com.ayanix

import android.os.Bundle
import android.widget.TextView
import androidx.appcompat.app.AppCompatActivity

class SignalActivity : AppCompatActivity() {

    lateinit var signalText: TextView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_signal)

        signalText = findViewById(R.id.signalText)

        // Dummy data example
        val emaFast = 1.123
        val emaSlow = 1.120
        val rsi = 30.0
        val candleBullish = true
        val volatilityOk = true

        val signal = SignalEngine.getSignal(emaFast, emaSlow, rsi, candleBullish, volatilityOk)
        signalText.text = signal
    }
}
[1/18, 10:42 PM] Ayan Butt: <?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical" android:layout_width="match_parent"
    android:layout_height="match_parent" android:padding="16dp">

    <Spinner
        android:id="@+id/marketSpinner"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"/>

    <Spinner
        android:id="@+id/timeSpinner"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" android:layout_marginTop="16dp"/>

    <Button
        android:id="@+id/startBtn"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="START" android:layout_marginTop="24dp"/>

</LinearLayout>
[1/18, 10:42 PM] Ayan Butt: <?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical" android:layout_width="match_parent"
    android:layout_height="match_parent" android:gravity="center"
    android:padding="16dp">

    <TextView
        android:id="@+id/signalText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="SIGNAL"
        android:textSize="32sp"
        android:textColor="#000000"/>
</LinearLayout>

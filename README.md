
public class MainActivity extends AppCompatActivity {
  private RewardedInterstitialAd rewardedInterstitialAd;
  private String TAG = "MainActivity";

  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);

    new Thread(
            () -> {
              // Initialize the Google Mobile Ads SDK on a background thread.
              MobileAds.initialize(this, initializationStatus -> {});
              // Load an ad on the main thread.
              runOnUiThread(
                  () -> {
                    loadAd();
                  });
            })
        .start();
  }

  public void loadAd() {
    // Use the test ad unit ID to load an ad.
    RewardedInterstitialAd.load(MainActivity.this, "ca-app-pub-3940256099942544/5354046379",
        new AdRequest.Builder().build(),  new RewardedInterstitialAdLoadCallback() {
      @Override
      public void onAdLoaded(RewardedInterstitialAd ad) {
        Log.d(TAG, "Ad was loaded.");
        rewardedInterstitialAd = ad;
      }
      @Override
      public void onAdFailedToLoad(LoadAdError loadAdError) {
        Log.d(TAG, loadAdError.toString());
        rewardedInterstitialAd = null;
      }
    });
  }
}

# Epic Games Integration

#### Links

- [Portal login](https://dev.epicgames.com/portal/blue-brain-games/products/dimhaventhelostsource)
- [Opensource EOS Plugin](https://github.com/EOS-Contrib/eos_plugin_for_unity)
- [EOS Plugin Implementation Guide](https://github.com/EOS-Contrib/eos_plugin_for_unity/blob/development/com.playeveryware.eos/Documentation~/getting_started_guide/README.md)
- [EOS Plugin Authentication Guide](https://github.com/EOS-Contrib/eos_plugin_for_unity/blob/stable/com.playeveryware.eos/Documentation~/player_authentication.md)

- [Achievements Overview](https://dev.epicgames.com/docs/epic-games-store/services/epic-achievements/overview)
- [EOS Integration Overview](https://dev.epicgames.com/docs/epic-online-services/eos-get-started/get-started-guide/integrate-eos-sdk)
- [Opensource EOS Plugin](https://github.com/EOS-Contrib/eos_plugin_for_unity)

#### EOS Portal Setup

GLHF

#### Code Examples

EosLauncherLogin.cs
```
        using System;
        using Epic.OnlineServices;
        using PlayEveryWare.EpicOnlineServices;
        using UnityEngine;
        using static PlayEveryWare.EpicOnlineServices.EOSManager;

        public class EosLauncherLogin : MonoBehaviour
        {
            // The Product User ID used by EOS Game Services,
            // including achievements.
            public ProductUserId LoggedInProductUserId { get; private set; }

            public bool IsLoggedIn =>
                LoggedInProductUserId != null &&
                LoggedInProductUserId.IsValid();

            // Other game systems can listen for successful login.
            public event Action<ProductUserId> LoginSucceeded;

            private void Start()
            {
                // EOSManager initializes the EOS platform interface.
                // Its GameObject must exist in the startup scene.
                if (EOSManager.Instance == null)
                {
                    Debug.LogError(
                        "EOSManager is missing. Add the plugin's " +
                        "Singletons prefab or EOSManager component."
                    );
                    return;
                }

                if (EOSManager.Instance.GetEOSPlatformInterface() == null)
                {
                    Debug.LogError(
                        "EOS platform interface is not initialized."
                    );
                    return;
                }

                // Retrieve the exchange code supplied by the Epic Games Launcher.
                var launcherArgs = EOSSingleton.GetCommandLineArgsFromEpicLauncher();

                string exchangeCode = launcherArgs.authPassword;

                if (string.IsNullOrWhiteSpace(exchangeCode))
                {
                    Debug.LogError(
                        "No Epic Games Launcher exchange code was found. " +
                        "Launch the game through the Epic Games Launcher, " +
                        "or use a separate development login flow."
                    );
                    return;
                }

                Debug.Log("Starting EOS Login " + exchangeCode);

                // STEP 1: Epic Account Auth login.
                EOSManager.Instance.StartLoginWithLoginTypeAndToken(
                    Epic.OnlineServices.Auth.LoginCredentialType.ExchangeCode,
                    null,
                    exchangeCode,
                    OnAuthLoginComplete
                );
            }

            private void OnAuthLoginComplete(
                Epic.OnlineServices.Auth.LoginCallbackInfo info)
            {
                if (info.ResultCode != Result.Success)
                {
                    Debug.LogError(
                        $"EOS Auth login failed: {info.ResultCode}"
                    );
                    return;
                }

                Debug.Log("EOS Auth login succeeded.");

                // STEP 2: Connect login using the authenticated
                // Epic Account ID.
                StartConnectLogin(info.LocalUserId);
            }

            private void StartConnectLogin(
                Epic.OnlineServices.EpicAccountId epicAccountId)
            {
                EOSManager.Instance.StartConnectLoginWithEpicAccount(
                    epicAccountId,
                    (Epic.OnlineServices.Connect.LoginCallbackInfo info) =>
                    {
                        if (info.ResultCode == Result.Success)
                        {
                            // Existing Connect user.
                            OnLoginReady(info.LocalUserId);
                            return;
                        }

                        if (info.ResultCode == Result.InvalidUser)
                        {
                            // First Connect login for this game.
                            // Create the player's Product User ID.
                            CreateConnectUser(
                                epicAccountId,
                                info.ContinuanceToken
                            );
                            return;
                        }

                        Debug.LogError(
                            $"EOS Connect login failed: {info.ResultCode}"
                        );
                    }
                );
            }

            private void CreateConnectUser(
                Epic.OnlineServices.EpicAccountId epicAccountId,
                Epic.OnlineServices.ContinuanceToken continuanceToken)
            {
                EOSManager.Instance.CreateConnectUserWithContinuanceToken(
                    continuanceToken,
                    (Epic.OnlineServices.Connect.CreateUserCallbackInfo info) =>
                    {
                        if (info.ResultCode != Result.Success)
                        {
                            Debug.LogError(
                                $"EOS Connect user creation failed: " +
                                $"{info.ResultCode}"
                            );
                            return;
                        }

                        Debug.Log("EOS Connect user created.");

                        // STEP 3: Retry Connect login after creating
                        // the user's Connect account.
                        StartConnectLogin(epicAccountId);
                    }
                );
            }

            private void OnLoginReady(ProductUserId productUserId)
            {
                if (productUserId == null || !productUserId.IsValid())
                {
                    Debug.LogError(
                        "Connect login succeeded without a valid ProductUserId."
                    );
                    return;
                }

                LoggedInProductUserId = productUserId;

                Debug.Log(
                    $"EOS login complete. Product User ID: {productUserId}"
                );

                // The game can now use EOS Game Services.
                LoginSucceeded?.Invoke(productUserId);
            }
        }
```

EosAchievements.cs

```
        using UnityEngine;
        using PlayEveryWare.EpicOnlineServices;
        using Epic.OnlineServices;
        using Epic.OnlineServices.Achievements;

        public static class EosAchievements
        {
            public static void Unlock(string achievementId)
            {
                // Get the EOS platform interface.
                var platform = EOSManager.Instance.GetEOSPlatformInterface();

                if (platform == null)
                {
                    Debug.LogError("EOS platform interface is not available.");
                    return;
                }

                // This MUST be a ProductUserId, not an EpicAccountId.
                var productUserId = EOSManager.Instance.GetProductUserId();

                if (productUserId == null || !productUserId.IsValid())
                {
                    Debug.LogError("No valid EOS ProductUserId.");
                    return;
                }

                var achievements = platform.GetAchievementsInterface();

                var options = new UnlockAchievementsOptions
                {
                    UserId = productUserId,
                    AchievementIds = new Utf8String[]
                    {
                        achievementId
                    }
                };

                achievements.UnlockAchievements(
                    ref options,
                    null,
                    OnUnlockComplete
                );
            }

            private static void OnUnlockComplete(
                ref OnUnlockAchievementsCompleteCallbackInfo data)
            {
                if (data.ResultCode == Result.Success)
                {
                    Debug.Log("EOS achievement unlocked.");
                }
                else
                {
                    Debug.LogError(
                        $"EOS achievement unlock failed: {data.ResultCode}"
                    );
                }
            }
        }
```

##### Setup

- Add **both** scripts to a DontDestroyOnLoad GameObject in the main menu
- Call `EosAchievements.Unlock(type.ToString());`, preferably from a try-catch

##### Building

- TODO

#Snapcall Android module#

## Getting Started ##

    Add it from your graddle :

      def urlFile = { url, name ->
        File file = new File("$buildDir/download/${name}.aar")
        file.parentFile.mkdirs()
        if (!file.exists()) {
          new URL(url).withInputStream { downloadStream ->
              file.withOutputStream { fileOut ->
                  fileOut << downloadStream
                  }
                }
                }
                files(file.absolutePath)
                }

            dependencies {
              implementation urlFile('https://sandbox.snapcall.io/android/1.0.0/snapcall_android_framework-1.1.0.aar', 'snapcall_android_framework')
            }

          - urlFile is a graddle function which download a aar file from a remote url and copy it to your build
            you can directly use the snapcall module after that.



## OS/Hardware requirements ##
    minSdkVersion set to 16 - and target/compileSdkVersion set to 27
    Created with Android Studio 3.1.0
    For any problem with the target or any problem with compilation in general please contact Snapcall.

## External Library ##
    -okhttp: Library for  WebSocket connection
      implementation 'com.squareup.okhttp3:okhttp:3.10.0'
    Snapcall come up with okhttp3 and you must add it to your project - if you use an other version of this library which raise conflict , please contact Snapcall.

##Import##
    import io.snapcall.snapcall_android_framework.*;

##Public Class Available for external package##
    Snapcall : class to manage the framework, launch and receive snapcall
    Snapcall_External_Parameter : class for custom attribute for snapcall

    Other public class are Activity or Service that are not designed to be used externaly

## Note

    There are no listener for snapcall event unlike for the IOS plateform due to the difference in the plateform aproach of the VOIP and the way how Android work.
    The voIP call is handled in an other process than your app use and the user can interact with it via an android Notification.
    This allow Snapcall to perform any task in voip without disrupt your app.
    The graphical element of snapcall do not appear in phone history/Recent Activity and your app will always come in first from it.

    The Snapcall SDK don't ask permission for microphone access itself unlike ios. - you need to ask it manually (SDK >= 23).
    Android permission need to be asked from an activity and the Snapcall Activity and the microphone capture of the Snapcall Service are totally unsynchronized.
    If you plane to receive call in your app  ask the permission before to receive any call and explain your user why he should accept it. If not snapcall will just reject call by throwing exception.

    For the Android push , that is not designed for voip purpose like ios one, there are high priority push with data and with normal notification to allow the app to wake up and get the push message

##Use##
    Snapcall :
        public static Snapcall  getInstance()
            Instantiate, if necessary, and return a Snapcall instance
                Snapcall.getInstance();

        public static void  releaseInstance()
            Release the Static reference to Snapcall
                Snapcall.releaseSnapcall();


        public void registerUser(@NonNull  String token, @Nullable String identifier, @Nullable String customClientIdentifier, @NonNull String applicationName,@Nullable final Snapcall_Listener snapcallIdentifierCallBack )
            Register or update an user for voip call application to application with the firebase token.
            Before you should send to snapcall your firebase cloud messaging applciation key to allow us to wake up your application.
            If you use this fonctionality you should call this function at every launch of your application.
                Snapcall.getInstance().registerUser(
                    token                       -> a FCM token to send a push
                    identifier                  -> the snapcall identifier for your user which can be used instead of a custom identifer
                    customClientIdentifier      -> String to link your user with snapcall with a custom String identifier
                    applicationName             -> the name you register in snapcall back office linked with your fcm key
                    snapcallIdentifierCallBack  -> reference to a class instance to had a callback on the succes of the function with the identifier as parameter
                )                               -> return true if success before the async request

        public void setUserActive(boolean state, @NonNull  String token, @Nullable String identifier, @Nullable String customClientIdentifier, @NonNull String applicationName , @Nullable final activate cb)
          function to change the status of one of your user(able to receive a call)
          Snapcall.getInstance().setUserActive(
            state                   -> boolean state of your user
            token                   -> Your FCM Token
            String identifier       -> Your Snapcall identifier
            customClientIdentifier  -> Your custom identifier
            applicationName         -> the name you register for your app
            activate                -> a Callback for the success of the request
                );

        final public void receiveCall(@NonNull Context context, Map message , @Nullable Snapcall_External_Parameter parameter) throws SnapcallException
            Launch the call with a push payload after the application receive a voip push notification.
            Should be call in the application delegate function inherite from PushKit didReceiveIncomingPushWith
                Snapcall.getInstance().receiveCall(
                context,                   -> Android Context of your Activity/application
                Map  ,                     -> the Map parameter in the FCM push
               parameter                   -> the Snapcall_external_Parameter you want to use for the call
                ) throws SnapcallException -> throw exception in case of error (like microphone access not accorded)

        final public void launchCall(@NonNull Context context, @NonNull String bidId, @Nullable Snapcall_External_Parameter parameter) throws SnapcallException
            Launch a Application to Application call with the snapcall identifier
                Snapcall.getInstance().launchCall(
                    Context                 -> context of your application (Activity/context)
                    bidId                  -> your identifier for the call in the snapcall Back Office
                    snapcallIdentifier     -> the snapcall identifier you get when you register an user
                    parameter              -> the Snapcall_external_Parameter you want to use for the call
                ) throws SnapcallException  -> throw exception in case of error (like microphone access not accorded)

        final public void launchCall(@NonNull Context context, @NonNull String bidId, @NonNull String applicationName,@NonNull String customClientIdentifier, @Nullable Snapcall_External_Parameter parameter) throws SnapcallException{
            Launch an Application to application call with your custom identifier
                Snapcall.getInstance().launchCall(
                    Context                   -> context of your application (Activity/context)
                    bidId :                     -> your identifier for the call in the snapcall Back Office
                    applicationName :           -> the name you register for your application
                    customClientIdentifier :    -> your custom identifier you register for an user
                    parameter :                 -> the Snapcall_external_Parameter you want to use for the call
                ) throws SnapcallException -> throw exception in case of error (like microphone access not accorded)


        final public void launchCall(@NonNull Context context, @NonNull String bidId, @Nullable Snapcall_External_Parameter parameter) throws SnapcallException {
            Launch a classic snapcall Call
                Snapcall.getInstance().launchCall(
                    context,   -> Android Context of your Activity/application
                    bidId,     -> your identifier for the call in the snapcall Back Office
                    parameter, -> the Snapcall_external_Parameter you want to use for the call
                )

       final public void restorCallUi(Context ctx)
            Function to retreive the user interface during a call
                 Snapcall.getInstance().restorCallUi(
                   Context -> Android Context of your Activity/application
                   );

      final public boolean isSnapcallRunning(Context ctx)
          Function to check if snapcall is running
            Snapcall.isSnapcallRunning(
                    Context -> Android Context of your Activity/application
              );

      final public String getPushDataParameter(Map message)
        function to get your custom data from push message
          Snapcall.getInstance.getPushDataParameter(
            Map   -> the push map gived buy FCM message
            )


        public final void askForPermissions(Activity ctx){
            Function to ask for the microphone permission
                Snapcall.getInstance().askForPermissions(
                  Activity   -> An activity instance
                )
        public final boolean permissionStatus(Context ctx){
            Function to get the status of the microphone permission
                Snapcall.getInstance().permissionStatus(
                Context   -> App/activity context
                ) -> return a boolean - true if permission accorded


    Snapcall_External_Parameter
        Instantiate
          Snapcall_External_Parameter parameter =  new Snapcall_External_Parameter();

        Members
          public String urlImage = null;                -> url of an image to use as background during the call
          public int textColor = 0;                     -> int which represent the Color of the text in snapcall UI
          public String callTitle  = null;              -> Title to display in the call UI
          public String displayName  = null;            -> Name to display in the call
          public String displayBrand = null;            -> Brand to diplay in the call
          public String senderName  = null;             -> Name to display in the remote app for App to App call
          public String senderBrand = null;             -> Brand to display in the remote app for App to App call
          public String externalContext = null;         -> the Context you want to store in snapcall api for a call
          public String pushTransfertData = null;       -> the data you want to send to other app in case App to App call
          public String notificationTittle = "Snapcall";-> the Android notification title when you wake up the app
          public String notificationBody = "";          -> the Android notification body when you wake up the app

        you can also add a icon in your asset for notification as a color, declare it in your manifest :

        <meta-data
            android:name="com.google.firebase.messaging.default_notification_icon"
            android:resource="@drawable/snapcall_icon_notif" />

        <meta-data
            android:name="com.google.firebase.messaging.default_notification_color"
            android:resource="@color/white" />


##Other Information##

    Firebase  -> https://firebase.google.com/docs/android/setup
        To receive call in app you should implement Firebase Push (Cloud messaging). You need to register on Firebase website and request for a app Key and send it to us the.
        in Your app you should implement class to receive Push event from google server and register your user with the key you got from.

        <service android:name="">
            <intent-filter>
                <action android:name="com.google.firebase.INSTANCE_ID_EVENT" />
            </intent-filter>
        </service>

        <service
            android:name="">
            <intent-filter>
                <action android:name="com.google.firebase.MESSAGING_EVENT"/>
            </intent-filter>
        </service>

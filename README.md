# AllVideoApp
All VideoApp

public class Utility {

    public static ArrayList<VideoInfo> getVideos(Activity activity) {
        ArrayList<VideoInfo> downloadedList = new ArrayList<>();
        downloadedList.clear();

        ContentResolver contentResolver = activity.getContentResolver();
        Uri uri = MediaStore.Video.Media.EXTERNAL_CONTENT_URI;

        Cursor cursor = contentResolver.query(uri, null, null, null, null);

        //looping through all rows and adding to list
        if (cursor != null && cursor.moveToFirst()) {
            do {

                @SuppressLint("Range")
                String title = cursor.getString(cursor.getColumnIndex(MediaStore.Video.Media.DISPLAY_NAME));
                @SuppressLint("Range")
                String duration = cursor.getString(cursor.getColumnIndex(MediaStore.Video.Media.DURATION));
                @SuppressLint("Range")
                String data = cursor.getString(cursor.getColumnIndex(MediaStore.Video.Media.DATA));

                if (title != null && duration != null && data != null)
                downloadedList.add(new VideoInfo(Formatter.formatShortFileSize(activity,new File(data).length())
                        ,data,timeConversion(Long.parseLong(duration)),title));

            } while (cursor.moveToNext());
        }

        return downloadedList;
    }

    @SuppressLint("DefaultLocale")
    public static String timeConversion(long value) {
        String videoTime;
        int dur = (int) value;
        int hrs = (dur / 3600000);
        int mns = (dur / 60000) % 60000;
        int scs = dur % 60000 / 1000;

        if (hrs > 0) {
            videoTime = String.format("%02d:%02d:%02d", hrs, mns, scs);
        } else {
            videoTime = String.format("%02d:%02d", mns, scs);
        }
        return videoTime;
    }
}

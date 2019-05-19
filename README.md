# NotePad
This is NotePad Android Application.
## 基于NotePad应用做功能扩展
## 基本功能
### （一）NoteList中显示条目增加时间戳显示
#### 1.在notelist_item.xml文件中加一个线性布局，再在其中加入时间的TextView布局
    <LinearLayout android:layout_height="match_parent"
        android:layout_width="match_parent"
        android:orientation = "vertical"
        xmlns:android="http://schemas.android.com/apk/res/android">
        <TextView xmlns:android="http://schemas.android.com/apk/res/android"
            android:id="@android:id/text1"
            android:layout_width="match_parent"
            android:layout_height="?android:attr/listPreferredItemHeight"
            android:textAppearance="?android:attr/textAppearanceLarge"
            android:gravity="center_vertical"
            android:paddingLeft="5dip"
            android:singleLine="true" />
        <TextView
            android:id="@+id/text2"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:textAppearance="?android:attr/textAppearanceLarge"
            android:textColor="#CD5C5C"
            android:textSize="20sp"/>
    </LinearLayout>
#### 2.在创建表时，增加一个字段
    @Override
    public void onCreate(SQLiteDatabase db) {
            db.execSQL("CREATE TABLE " + NotePad.Notes.TABLE_NAME + " ("
                   + NotePad.Notes._ID + " INTEGER PRIMARY KEY,"
                   + NotePad.Notes.COLUMN_NAME_TITLE + " TEXT,"
                   + NotePad.Notes.COLUMN_NAME_NOTE + " TEXT,"
                   + NotePad.Notes.COLUMN_NAME_CREATE_DATE + " INTEGER,"
                   + NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE + " INTEGER,"
                   + NotePad.Notes.COLUMN_NAME_BACK_COLOR + " INTEGER" //颜色
                   + ");");
    }
#### 3.在NoteList.java中修改PROJECTION，dataColumns，viewIDs，使其能够读出和显示
    private static final String[] PROJECTION = new String[] {
            NotePad.Notes._ID, // 0
            NotePad.Notes.COLUMN_NAME_TITLE, // 1
            NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE,
            NotePad.Notes.COLUMN_NAME_BACK_COLOR,
    };
    private String[] dataColumns = { NotePad.Notes.COLUMN_NAME_TITLE ,  NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE } ;
    private int[] viewIDs = { android.R.id.text1 , R.id.text2 };
#### 4.修改NotePadProvider.java中的insert函数
    Long now = Long.valueOf(System.currentTimeMillis());
    Date date = new Date(now);
    SimpleDateFormat format = new SimpleDateFormat("yy.MM.dd HH:mm:ss");
    String dateTime = format.format(date);
#### 5.修改NoteEditor.java中的updateNote函数
    ContentValues values = new ContentValues();
    Long now = Long.valueOf(System.currentTimeMillis());
    Date date = new Date(now);
    SimpleDateFormat format = new SimpleDateFormat("yy.MM.dd HH:mm:ss");
    String dateTime = format.format(date);
    values.put(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE, dateTime);
#### 运行结果截图如下：

<img width="200" src="https://github.com/dj-jun/NotePad-master/blob/master/pictures/21.png"/>

### （二）添加笔记查询功能（根据标题查询）


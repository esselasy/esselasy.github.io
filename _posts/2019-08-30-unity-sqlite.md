---
title: "유니티에서 sqlite 사용하기"
date: 2019-08-27 03:40:00 +0900
categories: 유니티
tags: sqlite
---

유니티 프로젝트 용으로 sqlite를 설치하고 사용하는 방법을 소개합니다.  
[How to setup SQLITE for Unity](https://ornithoptergames.com/how-to-set-up-sqlite-for-unity/)를 참고하여 정리하였습니다.

## SQLite 설치
### 1. 프로젝트의 Assets 아래에 Plugins/SQLite 폴더를 생성합니다.

### 2. 유니티용 SQLite 파일 준비  
Unity가 설치된 디렉토리 하위의 Editor/Data/Mono/lib/mono/2.0/ 디렉토리에서 다음 파일들을 Assets/Plugins/SQLite 폴더로 복사합니다.
```
-- Assets
    -- Plugins
      -- SQLite
          Mono.Data.Sqlite.dll
          Mono.Data.SqliteClient.dll 
```

### 3. 메뉴의 Edit > Player settings의 Other Settings에서 "Api Compatibility Level"을 2.0으로 선택합니다.

### 4. Editor/Standalone 지원
#### SQLite dll 다운로드  
SQLite 사이트의 [sqlite.org](https://sqlite.org/)의 [Download](https://sqlite.org/download.html)에 접속해서 "Precompiled Binaries for Windows"에 있는 sqlite-dll-win32-x86-3290000.zip과 sqlite-dll-win64-x64-3290000.zip 파일을 다운로드합니다. 3290000과 같은 버전은 다를 수 있습니다.
4. zip 파일을 풀어서 안에 있는 dll을 다음과 같이 디렉토리에 추가합니다.
```
-- Assets
  -- Plugins
      -- SQLite
        -- x64
            sqlite3.dll
        -- x86
            sqlite3.dll            
```

#### sqlite3.dll 설정  
x64/sqlite3.dll을 클릭해서 Inspector를 열어서 아래와 같이 설정합니다.
![](https://ornithoptergames.com/content/images/2017/11/sqlite-dll-x64-01.png)
![](https://ornithoptergames.com/content/images/2017/11/sqlite-dll-x64-02.png)

    x86/sqlite3.dll에 대해서도 똑같이 해줍니다.

### 5. iOS 지원
3번까지 적용하는 것으로 사용할 수 있습니다.

### 6. 안드로이드 지원
안드로이드에서 사용하려면 SQLite 소스를 빌드해야 합니다. 32bit만 사용하려면 ["SQLite and Unity: How to do it right."](https://medium.com/@rizasif92/sqlite-and-unity-how-to-do-it-right-31991712190)에서 이미 빌드된 SQLite를 받아서 사용해도 됩니다.

#### 안드로이드 NDK 설치
[NDK 시작하기](https://developer.android.com/ndk/guides/index.html)의 "NDK 및 도구 다운로드"를 따라 안드로이드 NDK를 설치합니다.

#### SQLite 소스 코드 다운로드
SQLite 사이트의 [sqlite.org](https://sqlite.org/)의 [Download](https://sqlite.org/download.html)에 접속해서 "Source Code"에 있는 sqlite-amalgamation-3290000.zip 파일을 다운로드합니다. 버전은 다를 수 있습니다.

#### NDK 프로젝트 생성
아래와 같이 NDK 프로젝트 폴더를 적당한 위치에 만듭니다. c 파일과 h 파일은 이전에 다운로드한 SQLite 소스 코드에 들어 있습니다.
```
-- (ndk project folder)
   -- jni
      shell.c
      sqlite3.c
      Android.mk
      -- include
         sqlite3.h
         sqlite3ext.h
```

#### Android.mk 파일 작성
다음과 같은 내용으로 Android.mk 파일을 생성합니다.
```
LOCAL_PATH := $(call my-dir)

include $(CLEAR_VARS)

LOCAL_LDLIBS := -llog

LOCAL_MODULE    := sqlite3
LOCAL_SRC_FILES := sqlite3.c

include $(BUILD_SHARED_LIBRARY)
```

#### ndk-build 실행
명령 프롬프트를 실행하고 jni 폴더로 이동해서 ndk-build를 실행합니다. ndk-build가 PATH에 들어있지 않을 경우 전체 경로를 적어줘야 합니다.
```
D:\Projects\Android\SQLiteNDK\jni>C:\Users\<username>\AppData\Local\Android\Sdk\ndk-bundle\build\ndk-build
```

NDK 프로젝트 폴더 밑에 다음과 같이 라이브러리가 생깁니다.
```
-- (ndk project folder)
   -- jni
   -- libs
      -- arm64-v8a
         libsqlite3.so
      -- armeabi-v7a
         libsqlite3.so
      -- x86
         libsqlite3.so
      -- x86_64
         libsqlite3.so         
```
#### Assets에 추가
libs 폴더를 Assets/Plugins/SQLite/Android로 이동합니다.
```
-- Assets
  -- Plugins
      -- SQLite
        -- Android
          -- libs
            -- arm64-v8a
               libsqlite3.so
            -- armeabi-v7a
               libsqlite3.so
            -- x86
               libsqlite3.so
```
안드로이드 x86_64는 유니티에서 지원하지 않으므로 삭제합니다.

#### Inspector 설정
모든 libsqlite3.so 파일을 클릭해서 플랫폼 Android를 선택하고, CPU를 각각 맞게 선택해줍니다.

![](https://ornithoptergames.com/content/images/2017/11/sqlite-lib-android.png)

## 예제

SQLiteExample.cs 스크립트를 만들고 빈 오브젝트를 생성해서 추가합니다.

```
using System.Data;
using Mono.Data.Sqlite;
using UnityEngine;

namespace ExampleProject {

	public class SQLiteExample : MonoBehaviour {

		private string dbPath;

		private void Start() {
			dbPath = "URI=file:" + Application.persistentDataPath + "/exampleDatabase.db";
			CreateSchema();
			InsertScore("GG Meade", 3701);
			InsertScore("US Grant", 4242);
			InsertScore("GB McClellan", 107);
			GetHighScores(10);
		}

		public void CreateSchema() {
			using (var conn = new SqliteConnection(dbPath)) {
				conn.Open();
				using (var cmd = conn.CreateCommand()) {
					cmd.CommandType = CommandType.Text;
					cmd.CommandText = "CREATE TABLE IF NOT EXISTS 'high_score' ( " +
					                  "  'id' INTEGER PRIMARY KEY, " +
					                  "  'name' TEXT NOT NULL, " +
					                  "  'score' INTEGER NOT NULL" +
					                  ");";

					var result = cmd.ExecuteNonQuery();
					Debug.Log("create schema: " + result);
				}
			}
		}

		public void InsertScore(string highScoreName, int score) {
			using (var conn = new SqliteConnection(dbPath)) {
				conn.Open();
				using (var cmd = conn.CreateCommand()) {
					cmd.CommandType = CommandType.Text;
					cmd.CommandText = "INSERT INTO high_score (name, score) " +
					                  "VALUES (@Name, @Score);";

					cmd.Parameters.Add(new SqliteParameter {
						ParameterName = "Name",
						Value = highScoreName
					});

					cmd.Parameters.Add(new SqliteParameter {
						ParameterName = "Score",
						Value = score
					});

					var result = cmd.ExecuteNonQuery();
					Debug.Log("insert score: " + result);
				}
			}
		}

		public void GetHighScores(int limit) {
			using (var conn = new SqliteConnection(dbPath)) {
				conn.Open();
				using (var cmd = conn.CreateCommand()) {
					cmd.CommandType = CommandType.Text;
					cmd.CommandText = "SELECT * FROM high_score ORDER BY score DESC LIMIT @Count;";

					cmd.Parameters.Add(new SqliteParameter {
						ParameterName = "Count",
						Value = limit
					});

					Debug.Log("scores (begin)");
					var reader = cmd.ExecuteReader();
					while (reader.Read()) {
						var id = reader.GetInt32(0);
						var highScoreName = reader.GetString(1);
						var score = reader.GetInt32(2);
						var text = string.Format("{0}: {1} [#{2}]", highScoreName, score, id);
						Debug.Log(text);
					}
					Debug.Log("scores (end)");
				}
			}
		}
	}

}
```

#### 결과

콘솔 화면에서 다음과 같은 결과를 확인할 수 있습니다.  
![](https://ornithoptergames.com/content/images/2017/11/scores-example.png)

#### 안드로이드 테스트

안드로이드 장치를 연결해서 빌드 후 실행한 뒤, 콘솔 로그를 확인합니다.  
콘솔 로그는 Android Studio에서 임의의 프로젝트를 하나 만들어서 실행하면 Logcat 탭에서 확인할 수 있습니다.
dll을 찾지 못하면 아래와 같은 에러가 발생합니다.  
```
E/Unity: Unable to find sqlite3
E/Unity: DllNotFoundException: sqlite3
        at (wrapper managed-to-native) Mono.Data.Sqlite.UnsafeNativeMethods.sqlite3_open_v2(byte[],intptr&,int,intptr)
      at Mono.Data.Sqlite.SQLite3.Open (System.String strFilename, Mono.Data.Sqlite.SQLiteOpenFlagsEnum flags, System.Int32 maxPoolSize, System.Boolean usePool) [0x00046] in <23b8fda6c7f14f18b977a088fd8f8c12>:0 
      at Mono.Data.Sqlite.SqliteConnection.Open () [0x0021a] in <23b8fda6c7f14f18b977a088fd8f8c12>:0 
      at ExampleProject.SQLiteExample.CreateSchema () [0x0000c] in <8781849a879a4fe6a2f668abf8de9acd>:0 
      at ExampleProject.SQLiteExample.Start () [0x0001a] in <8781849a879a4fe6a2f668abf8de9acd>:0 
```
apk 파일을 풀어서 lib/armeabi-v7a와 같은 폴더에 libsqlite3.so 파일이 있는지 확인합니다. 없을 경우 libsqlite3.so의 Inspector 설정을 점검해야 합니다.

## 참고 자료
[How to setup SQLITE for Unity](https://ornithoptergames.com/how-to-set-up-sqlite-for-unity/)  
[SQLite and Unity: How to do it right.](https://medium.com/@rizasif92/sqlite-and-unity-how-to-do-it-right-31991712190)

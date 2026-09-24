Writeup Conti CTF
Date: 20/9/2026

Platform: TryHackMe

Category: Blue Team / DFIR / Incident Response

Overview
Incident Investigation Report: Conti Ransomware Analysis.

Objective: Investigating a ransomware attack incident using log analysis techniques to uncover malicious activity, artifacts, and execution paths.

Key Tools Used: Splunk (SPL), Sysmon, Windows Event Logs.

Investigation Summary & Findings
Q1: Can you identify the location of the ransomware?
בשלב הראשוני, הפעלתי את מכונת המעבדה שעליה מותקן שרת ה-Splunk, התחברתי למערכת וביצעתי חיפוש ראשוני במטרה לאתר את הודעת הכופרה (ה-Ransom Note) שהשאירה קבוצת Conti.

![[Pasted image 20260920045157.png]]

לאחר שאיתרתי את הודעת הכופרה (readme.txt) שנוצרה במערכת, התחלתי לבדוק איזה תהליך אחראי ליצירתו. במטרה לאתר את הנתיב המדויק, ביצעתי חיפוש מתקדם בשפת SPL. סיננתי לפי קוד אירוע 11 (יצירת קובץ) ומיקדתי את טווח הזמנים:

Splunk SPL
index=* EventCode=11 readme.txt*
וכך מצאתי את המיקום על ידי ההצלבה בין קוד 11 (יצירת קובץ) לבין קובץ ה-readme.txt.

הסינון הזה נתן לי את הנתיב שבו נוצר הקובץ:

![[Pasted image 20260920045535.png]]

Q2: What is the Sysmon event ID for the related file creation event?
הקוד הוא 11 ליצירת קובץ כפי שתואר קודם.

Q3: Can you find the MD5 hash of the ransomware?
לאחר שגיליתי את מיקום התיקיה בה רצה התוכנה, ביצעתי סינון לקוד 1 של יצירת תהליך (EventCode=1) כדי לגלות את ה-MD5 Hash של הקובץ.

הבנתי שאירועי יצירת תהליך הם אלו שמתעדים את הרצת הקובץ בפועל ולכן מכילים את חתימות האבטחה וה-Hashes, בניגוד לאירועי יצירת קובץ רגילים.

לשם כך, הרצתי את החיפוש הבא ב-SPL:

Splunk SPL
index=* EventCode=1 Image="Administrator"
לאחר חיפוש בתוך שדות ה-Event הרלוונטי (תחת שדה ה-Hashes), מצאתי את ה-MD5 Hash של התוכנה הזדונית:

![[Pasted image 20260920051118.png]]

Q4: What file was saved to multiple folder locations?
הבנתי שחיפוש ניחושים עיוור של שמות קבצים אינו מקצועי, ולכן עלי לבצע חיפוש מובנה שמשלב את אירועי יצירת הקבצים יחד עם סינון חכם שיציג את הקבצים הנפוצים ביותר במערכת.

לשם כך, הפעלתי סינון לפי קוד אירוע 11 (יצירת קובץ) והוספתי פקודת top limit=30 לשדה של ה-TargetFilename, במטרה לגלות בצורה אובייקטיבית איזה קובץ בדיוק נשמר מספר רב של פעמים בתיקיות שונות. התוצאות הראו באופן חד-משמעי כי קובץ ה-readme.txt הוא הקובץ שחיפשתי.

לשם כך, הרצתי את החיפוש הבא ב-SPL:

Splunk SPL
index=* EventCode=11 | top limit=30 TargetFilename
![[Pasted image 20260920053438.png]]

Q5: What was the command the attacker used to add a new user to the compromised system?
חיפשתי את הפקודה שיוצרת משתמש (המחרוזת שקשורה ל-net user), תוך התמקדות באירועי יצירת תהליך ושורות הפקודה שלהם. לשם כך הרצתי את החיפוש הבא ב-SPL:

Splunk SPL
index=* EventCode=1 "net user"
בחיפוש בלוגים מצאתי את הפקודה המדויקת שהתוקף הריץ בפועל באמצעות net1.exe כדי להכניס את המשתמש החדש ולהגדיר לו גישה והרשאות:

DOS
net user /add securityninja hardToHack123$
![[Pasted image 20260922022013.png]]

Q6: The attacker migrated the process for better persistence. What is the migration target (full image path)?
הבנתי שפעולת הגירת תהליכים (Process Injection / Migration) מתבצעת כאשר תוקף מעביר קוד או פותח חוט ריצה (Thread) בתוך הזיכרון של תהליך אחר. כדי לאתר פעילות כזו, נעזרתי בטיפ וחיפשתי ב-SPL אירועי Sysmon Event ID 8 (CreateRemoteThread), המיועדים בדיוק לתיעוד מנגנונים אלו. בתוך אירועים אלו, התמקדתי בשדה ה-TargetImage המציג את התהליך המארח שאליו בוצעה ההזרקה.

ממצאים:

מתוך ניתוח אירועי יצירת ה-Thread מרחוק, איתרתי את הנתיב המלא של התהליך שאליו התוקף ביצע את ההגירה:

C:\Windows\System32\wbem\unsecapp.exe

![[Pasted image 20260922022611.png]]

Q7: The attacker also injected into a system process to retrieve the hashes. What is the target process image used for getting the system hashes?
בהמשך לשאלה הקודמת, מדובר על אותו לוג וההתייחסות פה היא לשדה TargetImage.

הנתיב המלא הוא:

C:\Windows\System32\lsass.exe

Q8: What is the web shell the exploit deployed to the system?
מכיוון ששרתי Exchange חשופים לרשת ומבוססים על שרת האינטרנט IIS, תוקפים נוהגים לשלוח בקשות HTTP מסוג POST כדי לשתול או להפעיל סקריפטים זדוניים בסביבת השרת. כדי לאתר פעילות זו, היה עלי לחקור את לוגי ה-IIS ב-Splunk בהתמקדות על שיטות POST.

תהליך החיפוש ב-SPL:

הרצתי שאילתה ממוקדת לסינון כל בקשות ה-POST שהגיעו לשרת האינטרנט:

Splunk SPL
index=* sourcetype=iis cs_method=POST
לאחר מכן, השתמשתי בפאנל ה-Interesting Fields של Splunk כדי לסנן ולבחון את סיומות הקבצים המעורבות בבקשות, והתמקדתי ב-cs_url_stem ובסיומת האופיינית לשרתי מיקרוסופט ושרתי Exchange (.aspx) שדרכה מתבצעת הפעלת קוד צד-שרת.

בעזרת הסינון המדויק, איתרתי את קובץ ה-WebShell החשוד שהתוקף העלה והריץ על המערכת:

WebShell File: i3gfPctK1c2x.aspx

![[Pasted image 20260924040648.png]]

Q9: What is the command line that executed this web shell?
לאחר זיהוי קובץ ה-WebShell החשוד עם סיומת aspx, המשכתי לחקור את לוגי ה-IIS כדי לראות אילו ערכים או פקודות הועברו בבקשות אל הקובץ הזה.

ביצעתי התאמה וחיפוש ישיר בלוגים תוך מיקוד בשדה ה-CommandLine (או שדה הבקשה המתאים בלוגי ה-IIS) וסינון לפי שם קובץ הסקריפט (aspx), בעזרת השאילתה:

Splunk SPL
index=* CommandLine=*aspx*
החיפוש הממוקד בלוגים הציג את הפקודה המדויקת שהורצה ונקשרה להפעלת ה-WebShell:

attrib.exe -r \\win-aoqkg2as2q7.bellybear.local\C$\Program Files\Microsoft\Exchange Server\V15\FrontEnd\HttpProxy\owa\auth\i3gfPctK1c2x.aspx

![[Pasted image 20260924040201.png]]

Q10: What three CVEs did this exploit leverage? Provide the answer in ascending order.
לאחר ניתוח הלוגים והבנת אופן הפעילות של התוקף, יש צורך להבין את החולשות המדויקות במערכת שאפשרו את חדירת הקוד הזדוני. במקרה זה, המתקפה עשתה שימוש בשילוב של פגיעויות גישה מרחוק וחולשות תשתית.

מתוך ממצאי החקירה והתחקיר של החדר, התגלה כי התוקפים ניצלו את שלושת ה-CVE-ים הבאים:

CVE-2018-13374 – חולשת Path Traversal / Arbitrary File Read במוצרי Fortinet (FortiOS SSL VPN), המאפשרת לתוקפים לקרוא קבצים רגישים ופרטי התחברות ללא אימות מוקדם.

CVE-2018-13379 – חולשת ניהול סשנים או שינוי סיסמאות במוצרי Fortinet, המשמשת לצד חולשות VPN כדי להשיג שליטה או לגנוב גישות משתמשים בלתי מורשות.

CVE-2020-0796 – חולשה קריטית לביצוע קוד מרחוק (RCE) המכונה "SMBGhost", הקיימת בפרוטוקול התקשורת SMBv3 של מערכות ההפעלה Windows, ומאפשרת לתוקפים להריץ קוד ברמת הליבה (Kernel) של השרת.
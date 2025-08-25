

## 1. المصادقة عبر الـ API التقليدي (رقم الهاتف وكلمة المرور)

هذه هي الطريقة الأساسية التي يستخدمها تطبيقك للمصادقة، حيث يتم إرسال بيانات الاعتماد مباشرة إلى الواجهة الخلفية (Backend API) للحصول على رمز مميز (Token) للمصادقة.

### أ. عملية إنشاء الحساب (Register)

**الهدف:** السماح للمستخدم بإنشاء حساب جديد باستخدام رقم هاتفه وكلمة مرور.

**الخطوات:**
1.  **الواجهة الأمامية (Frontend):** تجمع بيانات المستخدم (الاسم، رقم الهاتف، كلمة المرور، تأكيد كلمة المرور، ونوع المستخدم).<mcfile name="RegisterRequest.ts" path="libs/api/src/openapi/models/RegisterRequest.ts"></mcfile>
2.  **إرسال الطلب:** ترسل الواجهة الأمامية طلب `POST` إلى نقطة نهاية التسجيل في الـ API.
3.  **الواجهة الخلفية (Backend):** تتحقق من صحة البيانات، تقوم بتشفير كلمة المرور، وتنشئ حسابًا جديدًا للمستخدم في قاعدة البيانات. إذا نجحت العملية، تُرجع رمز مصادقة (مثل `plainTextToken`) وبيانات المستخدم.
4.  **الواجهة الأمامية:** تخزن رمز المصادقة هذا (عادةً في `localStorage` أو `AsyncStorage` أو `SecureStore`) وتستخدمه للمصادقة على الطلبات المستقبلية.

**نقطة نهاية الـ API:** `POST /api/auth/register`

**مثال على الطلب (Request Body):**
```json
{
  "name": "اسم المستخدم",
  "phone_number": "+966501234567",
  "password": "كلمة_مرور_قوية",
  "password_confirmation": "كلمة_مرور_قوية",
  "is_customer": true,
  "is_company_owner": false,
  "is_service_provider": false
}
```

**مثال على الكود (Fetch API):**
```javascript
const registerUser = async (userData) => {
  try {
    const response = await fetch('https://api.taklifa.com/api/auth/register', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json',
      },
      body: JSON.stringify(userData),
    });

    const data = await response.json();

    if (response.ok) {
      console.log('تم إنشاء الحساب بنجاح:', data);
      // تخزين رمز المصادقة (plainTextToken) وبيانات المستخدم
      localStorage.setItem('authToken', data.data.plainTextToken);
      localStorage.setItem('user', JSON.stringify(data.data.user));
      return data;
    } else {
      console.error('خطأ في إنشاء الحساب:', data);
      throw new Error(data.message || 'فشل إنشاء الحساب');
    }
  } catch (error) {
    console.error('حدث خطأ:', error);
    throw error;
  }
};

// مثال للاستخدام:
// registerUser({
//   name: 'أحمد', 
//   phone_number: '+966501234567', 
//   password: 'password123', 
//   password_confirmation: 'password123',
//   is_customer: true
// });
```

**مثال على الكود (Axios):**
```javascript
import axios from 'axios';

const registerUserAxios = async (userData) => {
  try {
    const response = await axios.post('https://api.taklifa.com/api/auth/register', userData, {
      headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json',
      },
    });

    console.log('تم إنشاء الحساب بنجاح:', response.data);
    // تخزين رمز المصادقة (plainTextToken) وبيانات المستخدم
    localStorage.setItem('authToken', response.data.data.plainTextToken);
    localStorage.setItem('user', JSON.stringify(response.data.data.user));
    return response.data;
  } catch (error) {
    console.error('خطأ في إنشاء الحساب:', error.response ? error.response.data : error.message);
    throw error.response ? error.response.data : error.message;
  }
};

// مثال للاستخدام:
// registerUserAxios({
//   name: 'أحمد', 
//   phone_number: '+966501234567', 
//   password: 'password123', 
//   password_confirmation: 'password123',
//   is_customer: true
// });
```

### ب. عملية تسجيل الدخول (Login)

**الهدف:** السماح للمستخدمين الحاليين بالوصول إلى حساباتهم.

**الخطوات:**
1.  **الواجهة الأمامية:** تجمع رقم الهاتف وكلمة المرور من المستخدم.<mcfile name="LoginRequest.ts" path="libs/api/src/openapi/models/LoginRequest.ts"></mcfile>
2.  **إرسال الطلب:** ترسل الواجهة الأمامية طلب `POST` إلى نقطة نهاية تسجيل الدخول في الـ API.
3.  **الواجهة الخلفية:** تتحقق من صحة بيانات الاعتماد. إذا كانت صحيحة، تُرجع رمز مصادقة (مثل `plainTextToken`) وبيانات المستخدم.
4.  **الواجهة الأمامية:** تخزن رمز المصادقة هذا وتستخدمه للمصادقة على الطلبات المستقبلية.

**نقطة نهاية الـ API:** `POST /api/auth/login`

**مثال على الطلب (Request Body):**
```json
{
  "phone_number": "+966501234567",
  "password": "كلمة_مرور_قوية"
}
```

**مثال على الكود (Fetch API):**
```javascript
const loginUser = async (credentials) => {
  try {
    const response = await fetch('https://api.taklifa.com/api/auth/login', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json',
      },
      body: JSON.stringify(credentials),
    });

    const data = await response.json();

    if (response.ok) {
      console.log('تم تسجيل الدخول بنجاح:', data);
      // تخزين رمز المصادقة (plainTextToken) وبيانات المستخدم
      localStorage.setItem('authToken', data.data.plainTextToken);
      localStorage.setItem('user', JSON.stringify(data.data.user));
      return data;
    } else {
      console.error('خطأ في تسجيل الدخول:', data);
      throw new Error(data.message || 'فشل تسجيل الدخول');
    }
  } catch (error) {
    console.error('حدث خطأ:', error);
    throw error;
  }
};

// مثال للاستخدام:
// loginUser({ phone_number: '+966501234567', password: 'password123' });
```

**مثال على الكود (Axios):**
```javascript
import axios from 'axios';

const loginUserAxios = async (credentials) => {
  try {
    const response = await axios.post('https://api.taklifa.com/api/auth/login', credentials, {
      headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json',
      },
    });

    console.log('تم تسجيل الدخول بنجاح:', response.data);
    // تخزين رمز المصادقة (plainTextToken) وبيانات المستخدم
    localStorage.setItem('authToken', response.data.data.plainTextToken);
    localStorage.setItem('user', JSON.stringify(response.data.data.user));
    return response.data;
  } catch (error) {
    console.error('خطأ في تسجيل الدخول:', error.response ? error.response.data : error.message);
    throw error.response ? error.response.data : error.message;
  }
};

// مثال للاستخدام:
// loginUserAxios({ phone_number: '+966501234567', password: 'password123' });
```

---

## 2. المصادقة الاجتماعية (Social Authentication) عبر Supabase (أو خدمات مشابهة)

تعتمد المصادقة الاجتماعية على بروتوكول OAuth 2.0، حيث يقوم مزود خدمة خارجي (مثل Google, Apple, Facebook) بالتحقق من هوية المستخدم، ثم يمنح تطبيقك الإذن بالوصول إلى معلومات المستخدم.

**الهدف:** السماح للمستخدمين بتسجيل الدخول/إنشاء حسابات باستخدام حساباتهم على منصات التواصل الاجتماعي.

**الخطوات (تدفق OAuth 2.0 - Authorization Code Flow):**

1.  **الواجهة الأمامية (Frontend):**
    *   عندما يضغط المستخدم على زر تسجيل الدخول الاجتماعي (مثلاً "Sign in with Google").
    *   تقوم الواجهة الأمامية بإعادة توجيه المستخدم إلى نقطة نهاية التفويض (Authorization Endpoint) الخاصة بمزود الخدمة (Google) أو خدمة المصادقة الوسيطة (مثل Supabase).
    *   رأينا هذا في الكود باستخدام `WebBrowser.openAuthSessionAsync` في <mcfile name="google-sign-in.native.tsx" path="s:\mopale app API laravel\taklifa-app\libs\features\auth\src\components\social-auth\google\google-sign-in.native.tsx"></mcfile>، حيث يتم توجيه المستخدم إلى `process.env.EXPO_PUBLIC_SUPABASE_URL/auth/v1/authorize`.

2.  **مزود الخدمة (Google/Apple/Facebook):**
    *   يُطلب من المستخدم تسجيل الدخول إلى حسابه على مزود الخدمة (إذا لم يكن مسجلاً بالفعل).
    *   يُطلب من المستخدم الموافقة على منح تطبيقك الأذونات المطلوبة (مثل الوصول إلى البريد الإلكتروني والاسم).

3.  **إعادة التوجيه مع `Authorization Code`:**
    *   بعد موافقة المستخدم، يقوم مزود الخدمة بإعادة توجيه المتصفح (أو التطبيق) إلى `redirect_uri` (عنوان URL لإعادة التوجيه) الذي تم تحديده مسبقًا في إعدادات تطبيقك لدى مزود الخدمة.
    *   يحتوي عنوان URL هذا على `Authorization Code` (رمز التفويض) كمعامل في الاستعلام (query parameter).
    *   **أين يتم تخزين `Authorization Code`؟** يتم إرسال `Authorization Code` **مؤقتًا** إلى الواجهة الأمامية (في عنوان URL لإعادة التوجيه). **لا يتم تخزينه بشكل دائم في الواجهة الأمامية.** دوره هو أن يتم استخدامه مرة واحدة فقط لتبادله برمز وصول.

4.  **الواجهة الخلفية (Supabase/Backend):**
    *   تتلقى الواجهة الخلفية (في هذه الحالة، Supabase) `Authorization Code` من الواجهة الأمامية.
    *   تقوم Supabase بتبادل هذا `Authorization Code` مع مزود الخدمة (Google) للحصول على `Access Token` (رمز الوصول) و `Refresh Token` (رمز التحديث) الخاص بالمستخدم.
    *   هذه العملية تحدث بين Supabase ومزود الخدمة مباشرةً، وهي آمنة لأن `Authorization Code` قصير الأجل ويُستخدم مرة واحدة فقط.
    *   تقوم Supabase بإنشاء أو تسجيل دخول المستخدم في نظامها، ثم تُصدر رمز جلسة (Session Token) خاص بها (عادةً JWT) وترسله إلى الواجهة الأمامية.

5.  **الواجهة الأمامية:**
    *   تتلقى الواجهة الأمامية رمز الجلسة (JWT) من Supabase.
    *   تقوم الواجهة الأمامية بتخزين رمز الجلسة هذا (مثلاً في `localStorage` أو `AsyncStorage` أو `SecureStore`) وتستخدمه للمصادقة على الطلبات المستقبلية إلى Supabase أو إلى الـ API الخاص بك إذا كان يتكامل مع Supabase.


# هنا امثلة ب react.js or next.js  

### إنشاء حساب (Register) في React

عند إنشاء حساب، سيرسل الواجهة الأمامية طلب `POST` إلى نقطة النهاية `/api/auth/register` مع بيانات المستخدم (رقم الهاتف، كلمة المرور، الاسم، ونوع المستخدم). إليك كيفية القيام بذلك باستخدام `fetch` و `axios`:

#### باستخدام `fetch`:

```javascript:/path/to/your/RegisterComponent.js
import React, { useState } from 'react';

const RegisterComponent = () => {
  const [name, setName] = useState('');
  const [phoneNumber, setPhoneNumber] = useState('');
  const [password, setPassword] = useState('');
  const [passwordConfirmation, setPasswordConfirmation] = useState('');
  const [isCustomer, setIsCustomer] = useState(true); // أو false حسب نوع المستخدم

  const handleRegister = async (e) => {
    e.preventDefault();
    try {
      const response = await fetch(`${process.env.NEXT_PUBLIC_API_URL}/api/auth/register`, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'Accept': 'application/json',
        },
        body: JSON.stringify({
          name,
          phone_number: phoneNumber,
          password,
          password_confirmation: passwordConfirmation,
          is_customer: isCustomer,
          // يمكنك إضافة is_company_owner أو is_service_provider هنا
        }),
      });

      const data = await response.json();

      if (response.ok) {
        console.log('تم إنشاء الحساب بنجاح:', data);
        // هنا يمكنك حفظ الرمز المميز (token) إذا تم إرجاعه وتوجيه المستخدم
        // localStorage.setItem('authToken', data.plainTextToken);
      } else {
        console.error('خطأ في إنشاء الحساب:', data.message || data.errors);
        // عرض رسائل الخطأ للمستخدم
      }
    } catch (error) {
      console.error('حدث خطأ غير متوقع:', error);
    }
  };

  return (
    <form onSubmit={handleRegister}>
      <input type="text" placeholder="الاسم" value={name} onChange={(e) => setName(e.target.value)} />
      <input type="text" placeholder="رقم الهاتف" value={phoneNumber} onChange={(e) => setPhoneNumber(e.target.value)} />
      <input type="password" placeholder="كلمة المرور" value={password} onChange={(e) => setPassword(e.target.value)} />
      <input type="password" placeholder="تأكيد كلمة المرور" value={passwordConfirmation} onChange={(e) => setPasswordConfirmation(e.target.value)} />
      <label>
        <input type="checkbox" checked={isCustomer} onChange={(e) => setIsCustomer(e.target.checked)} />
        أنا عميل
      </label>
      <button type="submit">إنشاء حساب</button>
    </form>
  );
};

export default RegisterComponent;
```

#### باستخدام `axios`:

```javascript:/path/to/your/RegisterComponent.js
import React, { useState } from 'react';
import axios from 'axios';

const RegisterComponent = () => {
  const [name, setName] = useState('');
  const [phoneNumber, setPhoneNumber] = useState('');
  const [password, setPassword] = useState('');
  const [passwordConfirmation, setPasswordConfirmation] = useState('');
  const [isCustomer, setIsCustomer] = useState(true);

  const handleRegister = async (e) => {
    e.preventDefault();
    try {
      const response = await axios.post(`${process.env.NEXT_PUBLIC_API_URL}/api/auth/register`, {
        name,
        phone_number: phoneNumber,
        password,
        password_confirmation: passwordConfirmation,
        is_customer: isCustomer,
      });

      console.log('تم إنشاء الحساب بنجاح:', response.data);
      // هنا يمكنك حفظ الرمز المميز (token) إذا تم إرجاعه وتوجيه المستخدم
      // localStorage.setItem('authToken', response.data.plainTextToken);
    } catch (error) {
      if (axios.isAxiosError(error) && error.response) {
        console.error('خطأ في إنشاء الحساب:', error.response.data.message || error.response.data.errors);
      } else {
        console.error('حدث خطأ غير متوقع:', error);
      }
    }
  };

  return (
    <form onSubmit={handleRegister}>
      <input type="text" placeholder="الاسم" value={name} onChange={(e) => setName(e.target.value)} />
      <input type="text" placeholder="رقم الهاتف" value={phoneNumber} onChange={(e) => setPhoneNumber(e.target.value)} />
      <input type="password" placeholder="كلمة المرور" value={password} onChange={(e) => setPassword(e.target.value)} />
      <input type="password" placeholder="تأكيد كلمة المرور" value={passwordConfirmation} onChange={(e) => setPasswordConfirmation(e.target.value)} />
      <label>
        <input type="checkbox" checked={isCustomer} onChange={(e) => setIsCustomer(e.target.checked)} />
        أنا عميل
      </label>
      <button type="submit">إنشاء حساب</button>
    </form>
  );
};

export default RegisterComponent;
```

### تسجيل الدخول (Login) في React

لتسجيل الدخول، سيرسل الواجهة الأمامية طلب `POST` إلى نقطة النهاية `/api/auth/login` مع رقم الهاتف وكلمة المرور. إليك كيفية القيام بذلك:

#### باستخدام `fetch`:

```javascript:/path/to/your/LoginComponent.js
import React, { useState } from 'react';

const LoginComponent = () => {
  const [phoneNumber, setPhoneNumber] = useState('');
  const [password, setPassword] = useState('');

  const handleLogin = async (e) => {
    e.preventDefault();
    try {
      const response = await fetch(`${process.env.NEXT_PUBLIC_API_URL}/api/auth/login`, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'Accept': 'application/json',
        },
        body: JSON.stringify({
          phone_number: phoneNumber,
          password,
        }),
      });

      const data = await response.json();

      if (response.ok) {
        console.log('تم تسجيل الدخول بنجاح:', data);
        // حفظ الرمز المميز (token) في التخزين المحلي (localStorage) أو في حالة التطبيق
        localStorage.setItem('authToken', data.plainTextToken);
        // توجيه المستخدم إلى الصفحة الرئيسية أو لوحة التحكم
      } else {
        console.error('خطأ في تسجيل الدخول:', data.message || data.errors);
        // عرض رسائل الخطأ للمستخدم
      }
    } catch (error) {
      console.error('حدث خطأ غير متوقع:', error);
    }
  };

  return (
    <form onSubmit={handleLogin}>
      <input type="text" placeholder="رقم الهاتف" value={phoneNumber} onChange={(e) => setPhoneNumber(e.target.value)} />
      <input type="password" placeholder="كلمة المرور" value={password} onChange={(e) => setPassword(e.target.value)} />
      <button type="submit">تسجيل الدخول</button>
    </form>
  );
};

export default LoginComponent;
```

#### باستخدام `axios`:

```javascript:/path/to/your/LoginComponent.js
import React, { useState } from 'react';
import axios from 'axios';

const LoginComponent = () => {
  const [phoneNumber, setPhoneNumber] = useState('');
  const [password, setPassword] = useState('');

  const handleLogin = async (e) => {
    e.preventDefault();
    try {
      const response = await axios.post(`${process.env.NEXT_PUBLIC_API_URL}/api/auth/login`, {
        phone_number: phoneNumber,
        password,
      });

      console.log('تم تسجيل الدخول بنجاح:', response.data);
      // حفظ الرمز المميز (token) في التخزين المحلي (localStorage) أو في حالة التطبيق
      localStorage.setItem('authToken', response.data.plainTextToken);
      // توجيه المستخدم إلى الصفحة الرئيسية أو لوحة التحكم
    } catch (error) {
      if (axios.isAxiosError(error) && error.response) {
        console.error('خطأ في تسجيل الدخول:', error.response.data.message || error.response.data.errors);
      } else {
        console.error('حدث خطأ غير متوقع:', error);
      }
    }
  };

  return (
    <form onSubmit={handleLogin}>
      <input type="text" placeholder="رقم الهاتف" value={phoneNumber} onChange={(e) => setPhoneNumber(e.target.value)} />
      <input type="password" placeholder="كلمة المرور" value={password} onChange={(e) => setPassword(e.target.value)} />
      <button type="submit">تسجيل الدخول</button>
    </form>
  );
};

export default LoginComponent;
```

### ملاحظات هامة:

*   **`process.env.NEXT_PUBLIC_API_URL`**: هذا المتغير البيئي يفترض أنك تستخدم Next.js أو بيئة React تدعم متغيرات البيئة التي تبدأ بـ `NEXT_PUBLIC_` أو `REACT_APP_`. تأكد من تعريف `NEXT_PUBLIC_API_URL` في ملف `.env.local` أو ما شابهه ليشير إلى عنوان URL الأساسي لواجهة برمجة التطبيقات الخاصة بك (على سبيل المثال، `http://localhost:8000`).
*   **إدارة الحالة (State Management)**: في الأمثلة، استخدمت `useState` لإدارة حالة حقول الإدخال. في تطبيق أكبر، قد تفكر في استخدام مكتبات إدارة الحالة مثل Redux أو Zustand أو Context API.
*   **التعامل مع الأخطاء**: الأمثلة تتضمن معالجة أساسية للأخطاء. في تطبيق حقيقي، ستحتاج إلى عرض رسائل خطأ واضحة للمستخدم.
*   **حفظ الرمز المميز (Token Storage)**: بعد تسجيل الدخول بنجاح، يتم حفظ `plainTextToken` في `localStorage`. هذا الرمز المميز (عادةً ما يكون JWT) يجب إرساله مع الطلبات اللاحقة إلى الواجهة الخلفية للمصادقة (عادةً في رأس `Authorization` كـ `Bearer Token`).
*   **التوجيه (Routing)**: بعد تسجيل الدخول أو إنشاء الحساب بنجاح، ستحتاج إلى توجيه المستخدم إلى صفحة أخرى (مثل لوحة التحكم). يمكنك استخدام `react-router-dom` لهذا الغرض.

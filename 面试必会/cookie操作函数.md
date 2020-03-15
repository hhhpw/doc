```js
const setCookie = (name, value, expirse, path, domain, secure) => {
//     const date  = new Date();
//     date.setTime(date.getTime() + (days * 24 * 60 * 60 * 1000));
//     documnet.cookie = name + "=" + encodeURLComponent(value) + ";" + ";expires=" + date.toGMTString();

  let cookieText = encodeURIComponent(name) + "=" + encodeURIComponent(value);

  if (expirse instanceof Date) {
    cookieText += "; expirse=" + expirse.toGMTString();
  }

  if (path) {
    cookieText += "; path=" + path;
  }

  if (domain) {
    cookieText += "; domain=" + domain;
  }

  if (secure) {
    cookieText += "; secure";
  }

  document.cookie = cookieText;
};

const getCookie = (cookie, name) => { 
  let cookieName = encodeURIComponent(name) + "=";
  let cookieStart = document.cookie.indexOf(cookieName);
  let cookieValue = null;

  if (cookieStart > -1) {
    // 存在
    let cookieEnd = document.cookie.indexOf(";", cookieStart);
    if (cookieEnd == -1) {
      // 末尾
      cookieEnd = document.cookie.length;
    }
    cookieValue = decodeURIComponent(document.cookie.substring(
      cookieStart + cookieName.length,
      cookieEnd))
  }
  return cookieValue;
};


const delCookie = (name, path, domain, secure) => {
   setCookie(name, "", new Date(0), path, domain, secure);
}

```
# CAPTCHA Solver  

[![Promo](https://github.com/bright-kr/LinkedIn-Scraper/raw/main/Proxies%20and%20scrapers%20GitHub%20bonus%20banner.png)](https://brightdata.co.kr/) 

[Bright Data's CAPTCHA Solver](https://brightdata.co.kr/products/web-unlocker/captcha-solver)을 사용하면 사용자 에뮬레이션, 핑거프린트 관리, 강력한 プロキシ 인프라를 통해 reCAPTCHA, hCaptcha, PX Captcha, GeeTest 등과 같은 CAPTCHA를 손쉽게 해결할 수 있습니다.  
CAPTCHA Solver는 [Scraping Browser](https://brightdata.co.kr/products/scraping-browser) 및 [Web Unlocker](https://brightdata.co.kr/products/web-unlocker)에 내장된 기능입니다.

맞춤 CDP 함수에 대해서는 [여기](https://docs.brightdata.com/scraping-automation/scraping-browser/cdp-functions/custom#captcha-solver)에서 자세히 알아보십시오.


## Features  

- 빠르고 자동화된 CAPTCHA 해결  
- reCAPTCHA, hCaptcha, PX Captcha, GeeTest, SimpleCaptcha 등과 호환  
- 탐지 우회를 위한 지능형 사용자 에뮬레이션 및 핑거프린팅  
- 수상 경력의 [100M+ IPs를 보유한 proxy network](https://brightdata.co.kr/proxy-types) 기반  
- 99.9% 업타임 및 24/7 지원과 함께 결과에 대해서만 비용을 지불  



## Why Choose CAPTCHA Solver  

- **전 세계 20,000+ 고객이 신뢰**  
- **개발자를 위해 구축**  
  - AI 기반 언락 로직  
  - 자동 CAPTCHA 해결 및 リトライ  
  - 내장 JavaScript 렌더링  
  - Puppeteer, Playwright, Selenium과 같은 도구와 손쉬운 통합
 
 > **📚 다음과 함께 Webスクレイピング에 대해 더 알아보십시오:**
 >> [**Puppeteer**](https://brightdata.co.kr/blog/how-tos/web-scraping-puppeteer)<br>
 >> [**Playwright**](https://brightdata.co.kr/blog/how-tos/playwright-web-scraping)<br>
 >> [**Selenium**](https://brightdata.co.kr/blog/how-tos/using-selenium-for-web-scraping)

- **비교할 수 없는 신뢰성**  
  - 99.9% 성공률  
  - 4년+ R&D 및 80명+ 전담 엔지니어  
  - 연간 5.5조 건 이상의 데이터 リクエスト 처리  



# How CAPTCHA Solver Works  

Bright Data의 CAPTCHA Solver는 **Scraping Browser** 및 **Web Unlocker**에 통합되어 기본적으로 **CAPTCHA를 자동으로 해결**합니다. 다음을 수행할 수 있습니다:  

- 코드에서 해결 프로세스를 모니터링  
- Chrome DevTools Protocol(CDP) 명령을 사용하여 CAPTCHA 해결 동작을 수동으로 토글  
- 필요 시 CAPTCHA 해결을 완전히 비활성화  



## **Automatic CAPTCHA Solving**  

`Captcha.solve` 명령을 사용하여 CAPTCHA를 자동으로 감지하고 해결하십시오. Python 버전은 [여기](https://docs.brightdata.com/scraping-automation/scraping-browser/cdp-functions/custom#captcha-solver)에서 확인할 수 있습니다.

### Command Overview  

```javascript
Captcha.solve({
    detectTimeout?: number // Timeout for CAPTCHA detection in milliseconds  
    options?: CaptchaOptions[] // Configuration options for CAPTCHA solving  
}) : SolveResult
```

### Example: NodeJS (Puppeteer)

```javascript
(async () => {
  const page = await browser.newPage();
  const client = await page.target().createCDPSession();
  await page.goto('https://site-with-captcha.com');
  try {
    // Automatically solve CAPTCHA  
    const { status } = await client.send('Captcha.solve', { detectTimeout: 30000 });
    console.log(`CAPTCHA solve status: ${status}`);
  } catch (error) {
    console.error('Error solving CAPTCHA:', error);
  }
})();
```

### Events Monitoring  

고급 사용 사례를 처리하기 위해 특정 CAPTCHA 해결 이벤트를 수신할 수 있습니다:  

- **`Captcha.detected`**: CAPTCHA가 감지되었고 해결이 시작됨  
- **`Captcha.solveFinished`**: CAPTCHA가 성공적으로 해결됨  
- **`Captcha.solveFailed`**: CAPTCHA 해결 실패  
- **`Captcha.waitForSolve`**: CAPTCHA Solver 완료를 대기 중  

#### NodeJS Example - Listening for Events

```javascript
const client = await page.target().createCDPSession();
await new Promise((resolve, reject) => {
  client.on('Captcha.solveFinished', (result) => {
    if (result.status === 'success') {
      resolve();
    } else {
      reject(new Error('CAPTCHA solving failed with status: ' + result.status));
    }
  });
  client.on('Captcha.solveFailed', () => reject(new Error('CAPTCHA solving failed')));
  setTimeout(() => reject(new Error('CAPTCHA solve timeout')), 300000); // Delay set to 5min, consider of changing it
});
```

## Manual CAPTCHA Management

완전한 제어가 필요하십니까? 동작을 구성하거나 해결을 완전히 비활성화하십시오.

### Disable Automatic CAPTCHA Solving

```javascript
Captcha.setAutoSolve({  
  autoSolve: false // Disable CAPTCHA solving  
});
```

### Disable CAPTCHA Auto-Solve for Specific Types

```javascript
Captcha.setAutoSolve({  
  autoSolve: true,  
  options: [{  
    type: 'usercaptcha', // Disable auto-solving for this CAPTCHA type  
    disabled: true  
  }]  
});
```

### Manually Solve CAPTCHAs

```javascript
(async () => {
  const page = await browser.newPage();
  const client = await page.target().createCDPSession();
  await client.send('Captcha.setAutoSolve', { autoSolve: false });
  await page.goto('https://site-with-captcha.com');
  try {
    const { status } = await client.send('Captcha.solve', { detectTimeout: 30000 });
    console.log('CAPTCHA solve status:', status);
  } catch (error) {
    console.error('Error solving CAPTCHA:', error);
  }
})();
```

## Supported CAPTCHA Types  

당사 솔버는 다음을 포함한 광범위한 CAPTCHA를 지원합니다:  

## Supported CAPTCHA Types  

당사 솔버는 다음을 포함한 광범위한 CAPTCHA를 지원합니다:  

- [**reCAPTCHA**](https://brightdata.co.kr/products/web-unlocker/captcha-solver/recaptcha)
- [**Click Captcha**](https://brightdata.co.kr/products/web-unlocker/captcha-solver/click-captcha)
- [**hCaptcha**](https://brightdata.co.kr/products/web-unlocker/captcha-solver/hcaptcha)
- [**PerimeterX**](https://brightdata.co.kr/products/web-unlocker/captcha-solver/perimeterx)
- [**SimpleCaptcha**](https://brightdata.co.kr/products/web-unlocker/captcha-solver/simplecaptcha)
- [**FunCaptcha**](https://brightdata.co.kr/products/web-unlocker/captcha-solver/funcaptcha)
- [**Cloudflare Turnstile**](https://brightdata.co.kr/products/web-unlocker/captcha-solver/cloudflare-turnstile)
- [**AWS WAF Captcha**](https://brightdata.co.kr/products/web-unlocker/captcha-solver/aws-waf-captcha)
- [**GeeTest CAPTCHA**](https://brightdata.co.kr/products/web-unlocker/captcha-solver/geetest-captcha)
- [**KeyCAPTCHA**](https://brightdata.co.kr/products/web-unlocker/captcha-solver/keycaptcha)
- [**Puzzle CAPTCHA**](https://brightdata.co.kr/products/web-unlocker/captcha-solver/puzzle-captcha)
- [**Yandex CAPTCHA**](https://brightdata.co.kr/products/web-unlocker/captcha-solver/yandex-captcha)
- [**Image CAPTCHA**](https://brightdata.co.kr/products/web-unlocker/captcha-solver/image-captcha)
- [**Text CAPTCHA**](https://brightdata.co.kr/products/web-unlocker/captcha-solver/text-captcha)

  

## Advanced Customization  

고급 설정을 사용하여 CAPTCHA 해결 로직을 세밀하게 조정하십시오.  

### Example: Custom Options for Cloudflare Challenges  

```javascript
const cfOptions = {
  timeout: 40000,
  selector: '#challenge-body-text, .challenge-form',
  check_timeout: 300,
  success_selector: '#challenge-success[style*=inline]',
  wait_networkidle: { timeout: 500 }
};
```

## Pricing  

| **Plan**          | **Price (1K Results)** | **Monthly Cost** | **Description**                                                                   |  
|--------------------|------------------------|------------------|-----------------------------------------------------------------------------------|  
| **Pay-as-you-go**  | $1.50                 | No commitment    | 비정기적 スクレイピング 요구에 이상적입니다.                                                 |  
| **Growth**         | $1.27                 | $499             | 확장 중인 팀에 맞춰 설계되었습니다.                                                       |  
| **Business**       | $1.12                 | $999             | 대규모 スクレイピング 운영에 적합합니다.                                     |  
| **Premium**        | $1.05                 | $1,999           | 미션 크리티컬 운영을 위한 우선 지원과 고급 기능을 제공합니다.         |  
| **Enterprise**     | Custom Quote          | Contact Us       | 맞춤 패키지, 프리미엄 SLA, 전담 Account Manager, SSO 및 개인화된 솔루션을 제공합니다. |  

🚀 **SPECIAL OFFER**: 첫 예치금을 최대 **$500**까지 달러-대-달러로 매칭해 드립니다!  


## Why Developers Love CAPTCHA Solver  

- **손쉬운 통합**: Puppeteer, Playwright, Selenium과 원활하게 동작합니다.  
- **고급 AI 기반 로직**: リトライ, CAPTCHA 해결, 핑거프린팅, IP 로테이션, 고급 ヘッダー를 자동으로 처리합니다.  
- **내장 브라우저**: JavaScript 렌더링을 위해 외부 브라우저를 관리할 필요가 없습니다.  
- **실시간 인사이트**: 라이브 대시보드를 통해 네트워크 성능을 모니터링하십시오.  
- **비교할 수 없는 지원**: 매일 새로운 기능이 추가되며 24/7 글로벌 고객 지원을 제공합니다.  


## FAQ  

### **How Does CAPTCHA Solver Work?**  
CAPTCHA Solver는 고급 AI 기반 로직을 사용하여 CAPTCHA를 자동으로 감지, 분석 및 해결합니다.  

### **Can It Handle Multiple CAPTCHAs Simultaneously?**  
예, 이 솔루션은 여러 CAPTCHA 유형을 同時接続으로 처리할 수 있도록 확장됩니다.  

### **What Happens If CAPTCHA Solving Fails?**  
자동으로 リトライ를 시도합니다. 문제가 지속되면 24/7 지원 팀에 문의하여 문제를 해결하십시오.  


**🌟 오늘 시작하고 CAPTCHA와 작별하십시오!**  

**[Start Free Trial](https://brightdata.co.kr/products/web-unlocker/captcha-solver)**
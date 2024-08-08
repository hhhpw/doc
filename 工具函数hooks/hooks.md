- 给特定时间，距离此时间的倒计时
```js

import dayjs from 'dayjs';
import utc from 'dayjs/plugin/utc'; // 用于处理 UTC 时间
import timezone from 'dayjs/plugin/timezone'; // 用于处理时区
import { useEffect, useRef, useState } from 'react';
dayjs.extend(utc);
dayjs.extend(timezone);

declare global {
  interface Window {
    workerTimer: any;
    interval_timer: number;
  }
}

interface TargetTime {
  targetH: number;
  targetM?: number;
  targetS?: number;
}

const useCountDown = (targetTime: TargetTime) => {
  dayjs.tz.setDefault('Asia/Shanghai');
  const timer = useRef(null);

  const { targetH, targetM, targetS } = targetTime;
  const [countdown, setCountdown] = useState({ h: '-', m: '-', s: '-' });

  const startCountDown = () => {
    timer.current = window.workerTimer.setInterval(() => {
      const now = dayjs();
      // 今天的targetH点
      let targetTimeToday = now
        .hour(targetH)
        .minute(targetM || 0)
        .second(targetS || 0);

      // 如果当前时间已超过今天的早上targetH点，则目标时间设置为明天早上targetH点
      if (now.isAfter(targetTimeToday)) {
        targetTimeToday = targetTimeToday.add(1, 'day');
      }

      const diff = targetTimeToday.diff(now);

      const hours = Math.floor(diff / 1000 / 60 / 60);
      const minutes = Math.floor((diff / 1000 / 60) % 60);
      const seconds = Math.floor((diff / 1000) % 60);

      setCountdown({
        h: hours.toString().padStart(2, '0'),
        m: minutes.toString().padStart(2, '0'),
        s: seconds.toString().padStart(2, '0'),
      });
    }, 1000);
  };

  useEffect(() => {
    window.workerTimer.clearInterval(timer.current);
    startCountDown();
    return () => {
      window.workerTimer.clearInterval(timer.current);
    };
  }, [targetTime]);

  return countdown;
};

export default useCountDown;

// 引入
const countdown = useCountDown({ targetH: 9, targetM: 45 });
const { h, m, s } = countdown;
```




- 动态去计算窗口大小 用于UI样式
```js
import { debounce } from '@/utils/tools';
import { useEffect, useState } from 'react';

const MAX_MOBILE_WIDTH = 768;
const MAX_PAD_WIDTH = 1200;

const useWindowSize = () => {
  const getSize = () => {
    return {
      width: window.innerWidth,
      height: window.innerHeight,
    };
  };
  const [windowSize, setWindowSize] = useState(getSize);
  const resizeHandler = () => {
    setWindowSize(getSize());
  };
  const debouncedResize = debounce(resizeHandler, 200);

  useEffect((): any => {
    resizeHandler();

    window.addEventListener('resize', debouncedResize);

    return () => {
      window.removeEventListener('resize', debouncedResize);
      return null;
    };
  }, []);

  return {
    ...windowSize,
    isMobileStyle: windowSize?.width <= MAX_MOBILE_WIDTH,
    isPadStyle: windowSize?.width <= MAX_PAD_WIDTH,
  };
};

export default useWindowSize;

```
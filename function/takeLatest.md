```ts

/**
 * 获取异步处理的最新结果
 * @param fn 用来异步处理的函数
 * @param abortError 请求中断时的错误
 * @returns 新的异步处理函数
 */
function takeLatest<T extends (...args: any[]) => Promise<any>>(
  fn: T,
  abortError?: string | number | Error
): T & { abort(reason?: string): void } {
  // 保存最新的promise的resolve
  let latestResolve: ((v: unknown) => void) | null;

  const latestFn = function (this: any, ...args: any[]) {
    return new Promise((resolve, reject) => {
      latestResolve = resolve;
      fn.apply(this, args).then((res) => {
        // 判断保存的resolve是不是当前的
        if (latestResolve === resolve) {
          latestResolve && latestResolve(res);
        } else {
          // 抛出被中断导致的异常
          reject(abortError || new Error("AbortError"));
        }
      }, reject);
    });
  } as any;

  latestFn.abort = () => {
    latestResolve = null;
  };

  return latestFn;
}

```
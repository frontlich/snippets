很多单位的进制是固定的，比如距离单位，米、千米，进制为1000，文件大小单位，b、kb、mb、tb，进制为1024，这种类型的数据格式化可以有一个通用的函数

```ts
const generateFormatter = (radix: number, units: string[]) => {
  return (value: number, handleValue?: (v: number) => number | string) => {
    const pow = Math.min(
      units.length - 1,
      Math.floor(Math.log(value) / Math.log(radix))
    );
    const num = value / Math.pow(radix, pow);
    return (
      (typeof handleValue === "function" ? handleValue(num) : num) + units[pow]
    );
  };
};

/** 格式化距离 */
const distance = generateFormatter(1000, ['m', 'km']);
/** 格式化文件大小 */
const fileSize = generateFormatter(1024, ['b', 'kb', 'mb', 'tb']);

fileSize(1023); // 1023b
fileSize(1024); // 1kb
fileSize(2000); // 1.953125kb
fileSize(2000, (v) => v.toFixed(2)); // 1.95kb
fileSize(2 * 1024); // 2kb
fileSize(2 * 1024 * 1024); // 2mb
fileSize(1025 * 1024 * 1024 * 1024); // 1025tb

```
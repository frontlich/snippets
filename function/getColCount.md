
金刚位的个数是不固定的，不同数量的金刚位对应不同的排版，此方法用于获取最佳排版的列数

```ts

/**
 * 获取排版的最佳列数
 * @param n 金刚位的个数
 * @param minCol 最小列数
 * @param maxCol 最大列数
 * @param maxRow 最大行数
 * @returns 列数
 */
const getColCount = (n: number, minCol: number, maxCol: number, maxRow?: number): number => {
  // 如果金刚位个数 > 最大列数乘以最大行数
  if (maxRow && n >= maxCol * maxRow) return maxCol;

  let min = maxCol, // 距离满排的最小差值
    col = 0; // 满排时的列数
  const loop = (mod: number) => {
    const rest = n % mod; // 当前列数对应的余数

    if (maxRow) {
      // 当前列数对应的最大行数
      const rows = Math.ceil(n / mod);
      // 如果超过最大行数
      if (rows > maxRow) return;
    }

    if (rest === 0) { // 满排
      col = mod;
      return;
    }

    min = Math.min(min, mod - rest); // 记录最小差值

    mod - 1 >= minCol && loop(mod - 1);
  };

  loop(maxCol);

  return col || getColCount(n + min, minCol, maxCol, maxRow);
};

```
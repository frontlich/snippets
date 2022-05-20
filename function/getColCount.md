
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
  if (maxRow && n >= maxCol * maxRow) return maxCol;

  let lack = maxCol, // 距离满排的最小差值
    col = 0;
  for (let i = maxCol; i >= minCol; i--) {
    const mod = n % i; // 当前列数对应的余数
    const curLack = mod ? i - mod : 0; // 当前距离满排的差值
    if ((!maxRow || Math.ceil(n / i) <= maxRow) && curLack < lack) { // 获取最小差值对应的col
      col = i;
      lack = curLack;
      if (lack === 0) break; // 当差值为0时，可终止循环
    }
  }

  return col;
};

```
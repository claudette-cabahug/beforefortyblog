## Understanding Matrixes And Submatrixes

```
function flippingMatrix(matrix) {
  let matrixL = matrix.length
  let submatrixL = matrix.length / 2
  let total = 0

  for (let r = 0; r < submatrixL; r++) {
    for (let c = 0; c < submatrixL; c++) {
      let max = Number.NEGATIVE_INFINITY 

      max = Math.max(matrix[r][c], max)    
      max = Math.max(matrix[r][matrixL - c - 1], max) 
      max = Math.max(matrix[matrixL - r - 1][c], max)  
      max = Math.max(matrix[matrixL - r - 1][matrixL - c - 1], max)  

      total += max
    }
  }

  return total
}
``` 

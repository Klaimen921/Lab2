using System;
using System.Collections.Generic;
using System.Text;

namespace _2._1
{
    class MyMatrix
    {
        protected double[,] matrix;
        private MyMatrix() { }
        private MyMatrix(int a, int b) 
        {
            matrix = new double[a, b];
        }
        public MyMatrix(MyMatrix that)
        {
            this.matrix = that.matrix;
        }
        public MyMatrix(double[,] matrix)
        {
            this.matrix = matrix;
        }
        public MyMatrix(double[][] that)
        {
            matrix = new double[that.GetLength(0), that[0].GetLength(0)];
            for (int i = 0; i < matrix.GetLength(0); i++)
            {
                for (int j = 0; j < matrix.GetLength(1); j++)
                {
                    matrix[i, j] = that[i][j];
                }
            }
        }
        public MyMatrix(string[] linearray)
        {
            char[] simvol = new char[] { ' ', '\t' };
            matrix = new double[linearray.GetLength(0), linearray[0].Split(simvol).GetLength(0)];
            for (int i = 0; i < matrix.GetLength(0); i++)
            {
                string[] line = linearray[i].Split(simvol,StringSplitOptions.RemoveEmptyEntries);
                for (int j = 0; j < matrix.GetLength(1); j++)
                {
                    matrix[i, j] = double.Parse(line[j]);
                }
            }
        }
        public MyMatrix(string allline)
        {
            string[] linearray = allline.Split('\n');
            char[] simvol = new char[] { ' ', '\t' };
            matrix = new double[linearray.GetLength(0), linearray[0].Split(simvol,StringSplitOptions.RemoveEmptyEntries).GetLength(0)];
            for (int i = 0; i < matrix.GetLength(0); i++)
            {
                string[] line = linearray[i].Split(simvol, StringSplitOptions.RemoveEmptyEntries);
                for (int j = 0; j < matrix.GetLength(1); j++)
                {
                    matrix[i, j] = double.Parse(line[j]);
                }
            }
        }
        public static MyMatrix operator +(MyMatrix one, MyMatrix two)
        {
            MyMatrix final = new MyMatrix(one.Height, one.Width);
            for (int i = 0; i < one.matrix.GetLength(0); i++)
            {
                for (int j = 0; j < one.matrix.GetLength(1); j++)
                {
                    final.matrix[i, j] =one.matrix[i,j]+ two.matrix[i, j];
                }
            }
            return final;
        }
        public static MyMatrix operator *(MyMatrix one, MyMatrix two)
        {
            MyMatrix final= new MyMatrix();
            final.matrix = new double[one.matrix.GetLength(0), two.matrix.GetLength(1)];
            for (int i = 0; i < final.matrix.GetLength(0); i++)
            {
                for (int j = 0; j < final.matrix.GetLength(1); j++)
                {
                    for (int k = 0; k < one.matrix.GetLength(1); k++)
                    {
                        final.matrix[i, j] += one.matrix[i, k] * two.matrix[k, j];
                    }
                }
            }
            return final;
        }
        public int Height
        {
            get 
            {
                return (matrix.GetLength(0)); 
            }
        }
        public int Width
        {
            get 
            {
                return (matrix.GetLength(1));
            }
        }
        public int GetHeight()
        {
            return (this.matrix.GetLength(0));
        }
        public int GetWidth()
        {
            return (this.matrix.GetLength(1));
        }
        public double this[int oneind, int twoind]
        {
            get 
            {
                return matrix[oneind, twoind]; 
            }
            set 
            {
                matrix[oneind, twoind] = value;
            }
        }
        public double GetIndex(int one, int two)
        {
            return (matrix[one, two]);
        }
        public void SetIndex(int one, int two, int value)
        {
            matrix[one, two] = value;
        }
        public override string ToString()
        {
            string line = "";
            for (int i = 0; i < matrix.GetLength(0); i++)
            {
                for (int j = 0; j < matrix.GetLength(1); j++)
                {
                    line += matrix[i, j] +"\t";
                }
                line += '\n';
            }
            return (line);
        }
        private double[,] GetTransponedArray()
        {
            double[,] final = new double[matrix.GetLength(1), matrix.GetLength(0)];
            for (int i = 0; i < this.matrix.GetLength(0); i++)
            {
                for (int j = 0; j < this.matrix.GetLength(1); j++)
                {
                    final[j, i] = matrix[i, j];
                }
            }
            return (final);
        }
        public MyMatrix GetTransponedCopy()
        {
            double[,] array = this.GetTransponedArray();
            return new MyMatrix(array);
        }
        public void TransponeMe()
        {
            this.matrix = GetTransponedArray();
        }
        static void Main(string[] args)
        {
            MyMatrix q1 = new MyMatrix(new double[3, 2] { { 1, 2 }, { 3, 4 }, { 5, 6 } });
            Console.WriteLine(q1);
            q1 = new MyMatrix(new double[2][] { new double[] { 1, 2, 3 }, new double[] { 4, 5, 6 } });
            Console.WriteLine(q1);
            MyMatrix q2 = new MyMatrix(new string[] { "1 2", "3    4", "5 6" });
            Console.WriteLine(q2);
            q2 = new MyMatrix("1 2 \n 3    4 \n 5 6");
            Console.WriteLine(q2);
            Console.WriteLine(q1 + q1);
            Console.WriteLine(q1 * q2);
            Console.WriteLine(q1.Height);
            Console.WriteLine(q1.Width);
            Console.WriteLine(q1.GetHeight());
            Console.WriteLine(q1.GetWidth());
            Console.WriteLine(q2.GetTransponedCopy());
            Console.WriteLine(q2);
            q2.TransponeMe();
            Console.WriteLine(q2);
        }
    }
}

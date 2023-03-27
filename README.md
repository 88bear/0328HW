# 0328HW

```cpp

#include <iostream>
#include <iomanip>
using namespace std;

class CMatrix{
private:
    int *m_Matrix;
    short m_Row;
    short m_Column;

public:
    CMatrix(int n = 0) : m_Row(n), m_Column(n){
        m_Matrix = new int [n*n] ();
    }

    CMatrix(int m, int n, int v = 0) : m_Row(m), m_Column(n){
        m_Matrix = new int [m*n] ();
        for(int i = 0; i < m*n; ++i){
            m_Matrix[i] = v;
        }
        // cout << "Constructor called for a " << m_Row << '*' << m_Column << " matrix." << '\n';
    }

    CMatrix(const CMatrix& B) : m_Row(B.Row()), m_Column(B.Column()){
        m_Matrix = new int [m_Row*m_Column]();
        for(int i = 0; i < m_Row*m_Column; ++i){
            m_Matrix[i] = B.Get(i / m_Column, i%m_Column);
        }
        // cout << "Copy Constructor called for a "<< m_Row << '*' << m_Column << " matrix." << '\n';
    }

    ~CMatrix(){
        delete[] m_Matrix;
        // cout << "Destructor called for a "<< m_Row << '*' << m_Column << " matrix." << '\n';
    }
    
    CMatrix& operator=(const CMatrix& other) {
        if(this != &other){
            delete[] m_Matrix;
            m_Row = other.m_Row;
            m_Column = other.m_Column;
            m_Matrix = new int[m_Row * m_Column]();
            for(int i = 0; i < m_Row * m_Column; ++i) {
                m_Matrix[i] = other.m_Matrix[i];
            }
        }
        return *this;
    }

    void Print(){
        int cnt = MaxEntry();
        for(int i = 0; i < m_Row; i++){
            for(int j = 0; j < m_Column; j++){
                cout << setw(cnt) << Get(i, j);
            }
            cout << '\n';
        }
    }

    int MaxEntry(){
        int max = 0; int cnt = 1;
        for(int i = 0; i < m_Row*m_Column; ++i){
            if(m_Matrix[i] > max) max = m_Matrix[i];
        }
        while(max > 0){
            max /= 10;
            cnt++;
        }
        return cnt;
    }

    int Row() const { return m_Row; }
    int Column() const { return m_Column; }
    
    int Get(int i, int j) const{
        return m_Matrix[i * m_Column + j];
    }

    void Set(int i, int j, int n){
        m_Matrix[i * m_Column + j] = n;
    }

    CMatrix Add(const CMatrix& B) const {
        if(m_Row != B.Row() || m_Column != B.Column()){
            return CMatrix(0);
        }
        CMatrix tmp(m_Row, m_Column, 0);
        for(int i = 0; i < m_Row; ++i){
            for(int j = 0; j < m_Column; ++j){
                tmp.Set(i, j, m_Matrix[i * m_Column + j] + B.Get(i, j));
            }
        }
        return tmp;
    }
    CMatrix Subtract(const CMatrix& B) const {
        if(m_Row != B.Row() || m_Column != B.Column()){
            return CMatrix(0);
        }
        CMatrix tmp(m_Row, m_Column, 0);
        for(int i = 0; i < m_Row; ++i){
            for(int j = 0; j < m_Column; ++j){
                tmp.Set(i, j, m_Matrix[i * m_Column + j] - B.Get(i, j));
            }
        }
        return tmp;
    }
    CMatrix Transpose() const {
        CMatrix result(m_Column, m_Row);
        for (int i = 0; i < m_Row; i++) {
            for (int j = 0; j < m_Column; j++) {
                result.Set(j, i, Get(i, j));
            }
        }
        return result;
    }

    CMatrix operator+(const CMatrix& B) const {
        return Add(B);
    }

    CMatrix operator-(const CMatrix& B) const {
        return Subtract(B);
    }

};

```
------------------
```cpp
#include <iostream>
#include <iomanip>
using namespace std;

class CMatrix{
private:
    int *m_Matrix;
    short m_Row;
    short m_Column;

public:
    CMatrix(int n = 0) : m_Row(n), m_Column(n){
        m_Matrix = new int [n*n] ();
    }

    CMatrix(int m, int n, int v = 0) : m_Row(m), m_Column(n){
        m_Matrix = new int [m*n] ();
        for(int i = 0; i < m*n; ++i){
            m_Matrix[i] = v;
        }
        // cout << "Constructor called for a " << m_Row << '*' << m_Column << " matrix." << '\n';
    }

    CMatrix(const CMatrix& B) : m_Row(B.Row()), m_Column(B.Column()){
        m_Matrix = new int [m_Row*m_Column]();
        for(int i = 0; i < m_Row*m_Column; ++i){
            m_Matrix[i] = B.Get(i / m_Column, i%m_Column);
        }
        // cout << "Copy Constructor called for a "<< m_Row << '*' << m_Column << " matrix." << '\n';
    }

    ~CMatrix(){
        delete[] m_Matrix;
        // cout << "Destructor called for a "<< m_Row << '*' << m_Column << " matrix." << '\n';
    }

    CMatrix& operator=(const CMatrix& other) {
        if(this != &other){
            delete[] m_Matrix;
            m_Row = other.m_Row;
            m_Column = other.m_Column;
            m_Matrix = new int[m_Row * m_Column]();
            for(int i = 0; i < m_Row * m_Column; ++i) {
                m_Matrix[i] = other.m_Matrix[i];
            }
        }
        return *this;
    }

    void Print(){
        int cnt = MaxEntry();
        for(int i = 0; i < m_Row; i++){
            for(int j = 0; j < m_Column; j++){
                cout << setw(cnt) << Get(i, j);
            }
            cout << '\n';
        }
    }

    int MaxEntry() const {
        int max = 0; int cnt = 1;
        for(int i = 0; i < m_Row*m_Column; ++i){
            if(m_Matrix[i] > max) max = m_Matrix[i];
        }
        if(max == 0)
            return 2;
        else{
            while(max > 0){    
            max /= 10;
            cnt++;
            }
            return cnt;
        }
    }
    int Row() const { return m_Row; }
    int Column() const { return m_Column; }
    
    int Get(int i, int j) const{
        return m_Matrix[i * m_Column + j];
    }

    void Set(int i, int j, int n){
        m_Matrix[i * m_Column + j] = n;
    }

    CMatrix Add(const CMatrix& B) const {
        if(m_Row != B.Row() || m_Column != B.Column()){
            return CMatrix(0);
        }
        CMatrix tmp(m_Row, m_Column, 0);
        for(int i = 0; i < m_Row; ++i){
            for(int j = 0; j < m_Column; ++j){
                tmp.Set(i, j, m_Matrix[i * m_Column + j] + B.Get(i, j));
            }
        }
        return tmp;
    }
    CMatrix Subtract(const CMatrix& B) const {
        if(m_Row != B.Row() || m_Column != B.Column()){
            return CMatrix(0);
        }
        CMatrix tmp(m_Row, m_Column, 0);
        for(int i = 0; i < m_Row; ++i){
            for(int j = 0; j < m_Column; ++j){
                tmp.Set(i, j, m_Matrix[i * m_Column + j] - B.Get(i, j));
            }
        }
        return tmp;
    }
    CMatrix Transpose() const {
        CMatrix result(m_Column, m_Row);
        for (int i = 0; i < m_Row; i++) {
            for (int j = 0; j < m_Column; j++) {
                result.Set(j, i, Get(i, j));
            }
        }
        return result;
    }

    CMatrix operator+(const CMatrix& B) const {
        return Add(B);
    }

    CMatrix operator-(const CMatrix& B) const {
        return Subtract(B);
    }

    friend ostream& operator<<(ostream& os, const CMatrix& mat){
        int cnt = mat.MaxEntry();
        for(int i = 0; i < mat.Row(); i++){
            for(int j = 0; j < mat.Column(); j++){
                os << setw(cnt) << mat.Get(i, j);
            }
            os << '\n';
        }
        return os;
    }

};
```

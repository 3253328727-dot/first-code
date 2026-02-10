
#include <stdio.h>
struct Student {
    char id[20];    
    char name[20];  
    float score[3]; 
    float avg;      
};
void input(struct Student stu[], int num);                 
void calcCourseAvg(struct Student stu[], int num, float course_avg[]); 
void findHighest(struct Student stu[], int num, int *stu_idx, int *course_idx, float *max_score); 
void output(struct Student stu[], int num, float course_avg[], int max_stu, int max_course, float max_score); 
void input(struct Student stu[], int num) {
    for (int i = 0; i < num; i++) {
        printf("请输入第%d名学生的信息：\n", i + 1);
        printf("学号：");
        scanf("%s", stu[i].id);
        printf("姓名：");
        scanf("%s", stu[i].name);
        printf("三门课成绩（空格分隔）：");
        scanf("%f %f %f", &stu[i].score[0], &stu[i].score[1], &stu[i].score[2]);
        stu[i].avg = (stu[i].score[0] + stu[i].score[1] + stu[i].score[2]) / 3.0;
    }
}
void calcCourseAvg(struct Student stu[], int num, float course_avg[]) {
    course_avg[0] = course_avg[1] = course_avg[2] = 0.0;
    for (int i = 0; i < num; i++) {
        course_avg[0] += stu[i].score[0];
        course_avg[1] += stu[i].score[1];
        course_avg[2] += stu[i].score[2];
    }
    course_avg[0] /= num;
    course_avg[1] /= num;
    course_avg[2] /= num;
}
void findHighest(struct Student stu[], int num, int *stu_idx, int *course_idx, float *max_score) {
    *max_score = stu[0].score[0];
    *stu_idx = 0;
    *course_idx = 0;
    for (int i = 0; i < num; i++) {
        for (int j = 0; j < 3; j++) {
            if (stu[i].score[j] > *max_score) {
                *max_score = stu[i].score[j];
                *stu_idx = i;
                *course_idx = j;
            }
        }
    }
}
void output(struct Student stu[], int num, float course_avg[], int max_stu, int max_course, float max_score) {
    FILE *fp = fopen("student_result.txt", "w");
    if (fp == NULL) {
        printf("文件打开失败！\n");
        return;
    }
    printf("\n===== 学生个人成绩 =====\n");
    fprintf(fp, "===== 学生个人成绩 =====\n");
    for (int i = 0; i < num; i++) {
        printf("第%d名学生：\n", i + 1);
        fprintf(fp, "第%d名学生：\n", i + 1);
        printf("学号：%s  姓名：%s\n", stu[i].id, stu[i].name);
        fprintf(fp, "学号：%s  姓名：%s\n", stu[i].id, stu[i].name);
        printf("成绩：%.2f、%.2f、%.2f  平均分：%.2f\n\n", 
               stu[i].score[0], stu[i].score[1], stu[i].score[2], stu[i].avg);
        fprintf(fp, "成绩：%.2f、%.2f、%.2f  平均分：%.2f\n\n", 
               stu[i].score[0], stu[i].score[1], stu[i].score[2], stu[i].avg);
    }
    printf("===== 每门课平均分 =====\n");
    fprintf(fp, "===== 每门课平均分 =====\n");
    printf("第1门课：%.2f\n第2门课：%.2f\n第3门课：%.2f\n\n", 
           course_avg[0], course_avg[1], course_avg[2]);
    fprintf(fp, "第1门课：%.2f\n第2门课：%.2f\n第3门课：%.2f\n\n", 
           course_avg[0], course_avg[1], course_avg[2]);
    printf("===== 最高分信息 =====\n");
    fprintf(fp, "===== 最高分信息 =====\n");
    printf("最高分：%.2f\n对应学生：%s（学号：%s）\n对应课程：第%d门课\n", 
           max_score, stu[max_stu].name, stu[max_stu].id, max_course + 1);
    fprintf(fp, "最高分：%.2f\n对应学生：%s（学号：%s）\n对应课程：第%d门课\n", 
           max_score, stu[max_stu].name, stu[max_stu].id, max_course + 1);
    fclose(fp);
    printf("\n结果已保存至 student_result.txt 文件！\n");
}
int main() {
    struct Student students[3];  
    float course_avg[3];         
    int max_stu_idx, max_course_idx; 
    float max_score;
    input(students, 3);               
    calcCourseAvg(students, 3, course_avg); 
    findHighest(students, 3, &max_stu_idx, &max_course_idx, &max_score); 
    output(students, 3, course_avg, max_stu_idx, max_course_idx, max_score);
    return 0;
}

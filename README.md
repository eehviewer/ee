 return score, comment
            
        elif q_type == 'true_false':
            # 判断题
            selected = user_answer lower() in ['true', '1', 't'] if user_answer else False
            is_correct = selected == question get('correct_answer', False)
            score = question['score'] if is_correct else 0
            return score, "正确" if is_correct else "错误"
            
        elif q_type in ['short_answer', 'essay']:
            # 简答题/论述题：关键词匹配或人工评分
            # 这里简化处理，实际应用中可能需要NLP技术
            if not user_answer:
                return 0, "未作答"
                
            # 简单关键词匹配示例
            keywords = question get('keywords', [])
            matched = sum(1 for kw in keywords if kw lower() in user_answer lower())
            match_ratio = matched / max(1, len(keywords))
            
            if match_ratio >= 0 8:
                score = question['score']
                comment = "优秀"
            elif match_ratio >= 0 5:
                score = round(question['score'] * 0 6, 2)
                comment = "良好"
            elif match_ratio > 0:
                score = round(question['score'] * 0 3, 2)
                comment = "需改进"
            else:
                score = 0
                comment = "未匹配到关键词"
                
            return score, comment
            
        else:
            return 0, "未知题型"

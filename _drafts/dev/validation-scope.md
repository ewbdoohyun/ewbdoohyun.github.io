

`하나의 contanct 중복되는 phone을 가질 수 없다.`

    validates :phone, uniqueness: { scope: :contact_id }


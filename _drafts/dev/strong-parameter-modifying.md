





    def item_params
        if params[:item][:instagram_codes]
              params.require(:item).permit( some params).merge(instagram_codes: params[:item][:instagram_codes].split(" ").map {|c| c.strip})
        else
              params.require(:item).permit( some params)
        end
    end